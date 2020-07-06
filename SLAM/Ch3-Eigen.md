# C++ Eigen Library

```c++
#include <iostream>
using namespace std;

#include<ctime>  // 核心部分
#include<Eigen/Core> //稠密函数的代数运算（逆，特征值等）
#include<Eigen/Dense>
using namespace Eigen;

#define MATRIX_SIZE 50

int main(int argc, char **argv) {
    // DEFINE A MATRIX
    Matrix<float, 2, 3> matrix_23; 
        // Eigen::Matrix(<typename Scalar, int RowsAtCompileTime, int ColsAtCompileTime>)
        // here build a 2*3 matrix, element type as float
    Vector3d v_3d;
        // Eigen::Vector3d == Eigen::Matrix<double, 3, 1>
    Matrix<float, 3, 1> vd_3d;

    Matrix3d matrix_33 = Matrix3d::Zero();
        // Eigen::Matrix3d == Eigen::Matrix<double, 3, 3>
        // here we initial all elements be 0;
    Matrix<double, Dynamic, Dynamic> matrix_dynamic;
        // dynamix matrix, where we dont know actual size 
    MatrixXd matrix_x;
        // Eigen::MatrixXd == Eigen::Matrix<double, Dynamic, Dynamic>

    // COMMON OPERATION
    // Initialization:
    matrix_23 << 1, 2, 3, 4, 5, 6;
    cout << "matrix 2*3 from 1 to 6: \n" << matrix_23 << endl;
    // get element from a matrix by sing ()
    cout << "print matrix 2*3: " << endl;
    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 3; ++j) {
            cout << matrix_23(i,j) << "\t";
        }
        cout << endl;
    }

    // multiplication of matrix and vector
    v_3d << 3, 2, 1;
    vd_3d << 4, 5, 6;

    // multiplication of two matrix should be same type:
    // also, the dimension of resultant matrix should be correct.
    // Matrix<double, 2, 1> wrong_type_result = matrix_23 * v_3d;
    Matrix<double, 2, 1> result = matrix_23.cast<double>()* v_3d;
    cout << "[1, 2, 3; 4, 5, 6] * [3, 2, 1]" << result.transpose() << endl;
    Matrix<float, 2, 1> result2 = matrix_23 * vd_3d;
    cout << "[1, 2, 3; 4, 5, 6] * [4, 5, 6]" << result2.transpose() << endl;
    
    // Matrix Operation
    matrix_33 = Matrix3d::Random();   // randomly initialize matrix
    cout << "random matrix: \n" << matrix_33 << endl;
    cout << "transpose: \n" << matrix_33.transpose() << endl; // transpose
    cout << "sum: " << matrix_33.sum() << endl; // summation of all elements
    cout << "trace: " << matrix_33.trace() << endl; // get trace
    cout << "times 10: \n" << 10 * matrix_33 << endl; // matrix multiple a constant
    cout << "inverse: \n" << matrix_33.inverse() << endl; // inverse of a matrix
    cout << "det: " << matrix_33.determinant() << endl; // determinant

    // Eigen value
    SelfAdjointEigenSolver<Matrix3d> eigen_solver(matrix_33.transpose() * matrix_33);
    cout << "Eigen values = \n" << eigen_solver.eigenvalues() << endl;
    cout << "Eigen vectors = \n" << eigen_solver.eigenvectors() << endl;

    // Solve a function
    Matrix<double, MATRIX_SIZE, MATRIX_SIZE> matrix_NN = 
        MatrixXd::Random(MATRIX_SIZE, MATRIX_SIZE);
    matrix_NN = matrix_NN * matrix_NN.transpose(); 
    Matrix<double, MATRIX_SIZE, 1> v_Nd = MatrixXd::Random(MATRIX_SIZE, 1);

    // 直接逆解
    clock_t time_stt = clock(); // record time
    Matrix<double, MATRIX_SIZE, 1> x = matrix_NN.inverse() * v_Nd;
    cout << "time of normal inverse is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

    // by using QR decomposition
    time_stt = clock();
    x = matrix_NN.colPivHouseholderQr().solve(v_Nd);
    cout << "time of QR decomposition is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

    // by using cholesky decomposition
    time_stt = clock();
    x = matrix_NN.ldlt().solve(v_Nd);
    cout << "time of ldlt decomposition is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

return 0;
}
```



# 3.6.1 Eigen几何模块数据演示

```c++
#include <iostream>
#include <cmath>
using namespace std;

#include <Eigen/Core>
#include <Eigen/Geometry>
using namespace Eigen;

int main(int argc, char **argv) {
    Matrix3d rotation_matrix = Matrix3d::Identity();
    AngleAxisd rotation_vector(M_PI / 4, Vector3d(0, 0, 1));
    cout.precision(3);
    cout << "rotation matrix =\n" << rotation_vector.matrix() << endl;

    rotation_matrix = rotation_vector.toRotationMatrix();
    Vector3d v(1, 0, 0);
    Vector3d v_rotated = rotation_vector * v;
    cout << "(1,0,0) after rotation (by angle axis) = " << v_rotated.transpose() << endl;

    v_rotated = rotation_matrix * v;
    cout << "(1,0,0) after rotation (by matrix) = " << v_rotated.transpose() << endl;

    Vector3d euler_angles = rotation_matrix.eulerAngles(2, 1, 0);
    cout << "yaw pitch roll = " << euler_angles.transpose() << endl;

    Isometry3d T = Isometry3d::Identity();
    T.rotate(rotation_vector);
    T.pretranslate(Vector3d(1,3,4));
    cout << "Transform matrix = \n" << T.matrix() << endl;

    Vector3d v_transformed = T * v;
    cout << "v transformed = " << v_transformed.transpose() << endl;

    Quaterniond q = Quaterniond(rotation_vector);
    cout << "quaternion from rotation vector = " << q.coeffs().transpose() << endl;

    q = Quaterniond(rotation_matrix);
    cout << "quaternion from rotation matrix = " << q.coeffs().transpose() << endl;

    v_rotated = q * v;
    cout << "(1,0,0) after rotation = " << v_rotated.transpose() << endl;
    cout << "should be equal to " << (q * Quaterniond(0, 1, 0, 0) * q.inverse()).coeffs().transpose() << endl;
    
    return 0;
}
```



# 3.6.2 坐标系变换

