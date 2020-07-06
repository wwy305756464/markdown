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
    // 普通的矩阵加减直接用+-符号就可以
    matrix_33 = Matrix3d::Random();   // randomly initialize matrix
    cout << "random matrix: \n" << matrix_33 << endl;
    cout << "transpose: \n" << matrix_33.transpose() << endl; // transpose
    cout << "sum: " << matrix_33.sum() << endl; // summation of all elements
    cout << "trace: " << matrix_33.trace() << endl; // get trace
    cout << "times 10: \n" << 10 * matrix_33 << endl; // matrix multiple a constant
    cout << "inverse: \n" << matrix_33.inverse() << endl; // inverse of a matrix
    cout << "det: " << matrix_33.determinant() << endl; // determinant

    // Eigen value
    // 实对称矩阵（Symmetrix Matrix）可以保证对角化成功
    // 对角化：除了对角线上有数字，矩阵中其余元素都为0
    SelfAdjointEigenSolver<Matrix3d> eigen_solver(matrix_33.transpose() * matrix_33);
    cout << "Eigen values = \n" << eigen_solver.eigenvalues() << endl;
    cout << "Eigen vectors = \n" << eigen_solver.eigenvectors() << endl;

    // 如何求解一个方程中的matrix？
    // 求解 matrix_NN * x = v_Nd 中的 x， N 的值是随机生成的
    Matrix<double, MATRIX_SIZE, MATRIX_SIZE> matrix_NN = 
        MatrixXd::Random(MATRIX_SIZE, MATRIX_SIZE);
    matrix_NN = matrix_NN * matrix_NN.transpose(); // 保证半正定（semi-positive definite）
    Matrix<double, MATRIX_SIZE, 1> v_Nd = MatrixXd::Random(MATRIX_SIZE, 1);

    // 直接逆解
    clock_t time_stt = clock(); // record time
    Matrix<double, MATRIX_SIZE, 1> x = matrix_NN.inverse() * v_Nd; // Ax=B -> x=A^(-1)B
    cout << "time of normal inverse is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

    // by using QR decomposition
    time_stt = clock();
    x = matrix_NN.colPivHouseholderQr().solve(v_Nd); // 用QR分解来求
    cout << "time of QR decomposition is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

    // by using cholesky decomposition
    time_stt = clock();
    x = matrix_NN.ldlt().solve(v_Nd);  // 用cholesky的方法求解，有递归性质
    cout << "time of ldlt decomposition is " << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
    cout << "x = " << x.transpose() << endl;

return 0;
}
```



# 3.6.1 Eigen几何模块数据演示

## Parameters

* 旋转矩阵（3*3）：`Eigen::Matrix3d`
* 旋转向量（3*1）：`Eigen::AngleAxisd`
* 欧拉角（3*1）：`Eigen::Vector3d`
* 四元数（4*1）：`Eigen::Quaterniond`
* 欧式变换矩阵（4*4）：`Eigen::Isometry3d`
* 仿射变换（4*4）：`Eigen::Affine3d`
* 射影变换（4*4）`Eigen::Projective3d`

## Code

```c++
#include <iostream>
#include <cmath>
using namespace std;

#include <Eigen/Core>
#include <Eigen/Geometry>
using namespace Eigen;

int main(int argc, char **argv) {
    // 旋转/平移的表示
    // 这里3D旋转矩阵直接用的 Eigen::Matrix3d
    Matrix3d rotation_matrix = Matrix3d::Identity();
    // 旋转向量用的是AngleAxis,它底层不直接是Matrix，但是在运算中可以当作矩阵使用，因为重载了运算符
    // AngleAxisd 代表是 double的AngleAxis, 3D rotation  as an angle + axis
   	// AngleAxis<double> aa(angle_in_radian, Vector3d(ax, ay, az))
    // M_PI是pi的宏定义
    AngleAxisd rotation_vector(M_PI / 4, Vector3d(0, 0, 1)); // 沿z轴旋转45度
    cout.precision(3);
    cout << "rotation matrix =\n" << rotation_vector.matrix() << endl; // 这里在vector后面用.matrix()将它转换成矩阵
    rotation_matrix = rotation_vector.toRotationMatrix();
    
    // 用AngleAxis进行坐标变换
    Vector3d v(1, 0, 0);
    Vector3d v_rotated = rotation_vector * v;
    cout << "(1,0,0) after rotation (by angle axis) = " << v_rotated.transpose() << endl;
	// 或者用旋转矩阵进行坐标变换
    v_rotated = rotation_matrix * v;
    cout << "(1,0,0) after rotation (by matrix) = " << v_rotated.transpose() << endl;

    // 欧拉角
    Vector3d euler_angles = rotation_matrix.eulerAngles(2, 1, 0); // 用ZYX顺序的欧拉角
    cout << "yaw pitch roll = " << euler_angles.transpose() << endl;
	
    // 用Eigen::Isometry进行欧式变换矩阵
    Isometry3d T = Isometry3d::Identity();   // 尽管这里是3D，但其实是4*4的矩阵
    T.rotate(rotation_vector);			    // 按照之前定义的rotation_vector进行旋转
    T.pretranslate(Vector3d(1,3,4));         // 将平移向量 t 设置为（1，3，4）
    cout << "Transform matrix = \n" << T.matrix() << endl;

    // 用变换矩阵进行坐标变换
    Vector3d v_transformed = T * v;		// 相当于 R*v+t
    cout << "v transformed = " << v_transformed.transpose() << endl;  
	
    // 仿射变换：Eigen::Affine3d
    // 射影变换：Eigen::Projective3d
    
    // 四元数
    Quaterniond q = Quaterniond(rotation_vector); // 将AngleAxis直接赋值给四元数
    cout << "quaternion from rotation vector = " << q.coeffs().transpose() << endl;
	// 这里coeffs()的顺序为（x,y,z,w), x,y,z是虚部，w是实部
    
    q = Quaterniond(rotation_matrix); // 将旋转矩阵赋值给四元数
    cout << "quaternion from rotation matrix = " << q.coeffs().transpose() << endl;

    // 使用四元数来旋转一个向量，这时我们用到了重载的乘法
    v_rotated = q * v; // qvq^{-1}
    cout << "(1,0,0) after rotation = " << v_rotated.transpose() << endl;
    // 不用重载的乘法，而是常规的向量乘法
    cout << "should be equal to " << (q * Quaterniond(0, 1, 0, 0) * q.inverse()).coeffs().transpose() << endl;
    
    return 0;
}
```



# 3.6.2-3.7 坐标系变换，沿轨迹移动的可视化演示

## 坐标转换

这里我们举出这个例子：假设有两个机器人在世界坐标系中。将世界坐标系记作W，机器人的坐标系分别为$R_1$和$R_2$。第一个机器人的位姿为$q_1=[0.35,0.2,0.3,0.1]^T$，$t_1=[0.3,0.1,0.1]^T$。第二个机器人的位姿为$q_2=[-0.5,0.4,-0.1,0.2]^T$，$t_2=[-0.1,0.5,0.3]^T$。这里的$ q,t $表达的是$T_{R_k,w},k=1,2$，也就是世界坐标系到相机坐标系的变换关系。现在的话，第一个机器人看到某个点在自身的坐标系下的坐标为$p_{R_1}=[0.5,0,0.2]^T$，我们想求该向量在第二个机器人的坐标系下的坐标

### code

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <Eigen/Core>
#include <Eigen/Geometry>

using namespace std;
using namespace Eigen;

int main(int argc, char** argv) {
    Quaterniond q1(0.35, 0.2, 0.3, 0.1), q2(-0.5, 0.4, -0.1, 0.2); // 两个机器人的位姿用四元数表示
    q1.normalize(); 
    q2.normalize(); // 四元数需要normalize之后才能使用，归一化
    Vector3d t1(0.3, 0.1, 0.1), t2(-0.1, 0.5, 0.3); // 定义两个机器人的位移值
    Vector3d p1(0.5, 0, 0.2); // 定义第一个机器人在world coordinate下的坐标

    Isometry3d T1w(q1), T2w(q2);
    T1w.pretranslate(t1);
    T2w.pretranslate(t2);

    Vector3d p2 = T2w * T1w.inverse() * p1;
    cout << endl << p2.transpose() << endl;
    return 0;
}
```

计算在第二个机器人坐标系下的坐标：
$$
p_{R_2}=T_{R_2,W}T_{W,R_1}p_{R_1}
$$

## 可视化演示

​	先假设我们储存了一个机器人的运动轨迹，想将其画到一个窗口中。这里轨迹文件存在了trajectory.txt中，由无数行组成，每一行记录了一个时间点的数据，格式为：
$$
time,t_x,t_y,t_z,q_x,q_y,q_z,q_w
$$
​	这里$time$是该位姿的记录时间，$t$是平移的量，$q$是旋转四元数，这几个量都是以世界坐标系到机器人坐标系记录。如果要存储机器人的轨迹，那么我们需要存储所有时刻的$T_{WR}$或者$T_{RW}$.

​	在画轨迹的时候，我们可以将其画成一系列点组成的序列，而这些点就是机器人（相机）坐标系的原点在世界坐标系中的坐标。如果$O_R$是机器人坐标系的原点，那么$O_W$就是这个原点在世界坐标系下的坐标：
$$
O_W=T_{WR}O_R=t_{WR}
$$
​	从上述式子中我们发现$O_W$是$T_{WR}$的平移部分，我们可以从中看到相机在哪里。

### code

```c++
#include <pangolin/pangolin.h>
#include <Eigen/Core>
#include <unistd.h>

// 本例演示了如何画出一个预先存储的轨迹

using namespace std;
using namespace Eigen;

// path to trajectory file
string trajectory_file = "./coordinateExamples/trajectory.txt";

void DrawTrajectory(vector<Isometry3d, Eigen::aligned_allocator<Isometry3d>>); 
// 声明之后要调用的函数

int main(int argc, char **argv) {
  vector<Isometry3d, Eigen::aligned_allocator<Isometry3d>> poses;
  ifstream fin(trajectory_file);
  if (!fin) {
    cout << "cannot find trajectory file at " << trajectory_file << endl;
    return 1;
  }

  while (!fin.eof()) {
    double time, tx, ty, tz, qx, qy, qz, qw;
    fin >> time >> tx >> ty >> tz >> qx >> qy >> qz >> qw;
    Isometry3d Twr(Quaterniond(qw, qx, qy, qz));
    Twr.pretranslate(Vector3d(tx, ty, tz));
    poses.push_back(Twr);
  }
  cout << "read total " << poses.size() << " pose entries" << endl;

  // draw trajectory in pangolin
  DrawTrajectory(poses);
  return 0;
}

/*******************************************************************************************/
void DrawTrajectory(vector<Isometry3d, Eigen::aligned_allocator<Isometry3d>> poses) {
  // create pangolin window and plot the trajectory
  pangolin::CreateWindowAndBind("Trajectory Viewer", 1024, 768);
  glEnable(GL_DEPTH_TEST);
  glEnable(GL_BLEND);
  glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);

  pangolin::OpenGlRenderState s_cam(
    pangolin::ProjectionMatrix(1024, 768, 500, 500, 512, 389, 0.1, 1000),
    pangolin::ModelViewLookAt(0, -0.1, -1.8, 0, 0, 0, 0.0, -1.0, 0.0)
  );

  pangolin::View &d_cam = pangolin::CreateDisplay()
    .SetBounds(0.0, 1.0, 0.0, 1.0, -1024.0f / 768.0f)
    .SetHandler(new pangolin::Handler3D(s_cam));

  while (pangolin::ShouldQuit() == false) {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    d_cam.Activate(s_cam);
    glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
    glLineWidth(2);
    for (size_t i = 0; i < poses.size(); i++) {
      // 画每个位姿的三个坐标轴
      Vector3d Ow = poses[i].translation();
      Vector3d Xw = poses[i] * (0.1 * Vector3d(1, 0, 0));
      Vector3d Yw = poses[i] * (0.1 * Vector3d(0, 1, 0));
      Vector3d Zw = poses[i] * (0.1 * Vector3d(0, 0, 1));
      glBegin(GL_LINES);
      glColor3f(1.0, 0.0, 0.0);
      glVertex3d(Ow[0], Ow[1], Ow[2]);
      glVertex3d(Xw[0], Xw[1], Xw[2]);
      glColor3f(0.0, 1.0, 0.0);
      glVertex3d(Ow[0], Ow[1], Ow[2]);
      glVertex3d(Yw[0], Yw[1], Yw[2]);
      glColor3f(0.0, 0.0, 1.0);
      glVertex3d(Ow[0], Ow[1], Ow[2]);
      glVertex3d(Zw[0], Zw[1], Zw[2]);
      glEnd();
    }
    // 画出连线
    for (size_t i = 0; i < poses.size(); i++) {
      glColor3f(0.0, 0.0, 0.0);
      glBegin(GL_LINES);
      auto p1 = poses[i], p2 = poses[i + 1];
      glVertex3d(p1.translation()[0], p1.translation()[1], p1.translation()[2]);
      glVertex3d(p2.translation()[0], p2.translation()[1], p2.translation()[2]);
      glEnd();
    }
    pangolin::FinishFrame();
    usleep(5000);   // sleep 5 ms
  }
}
```

