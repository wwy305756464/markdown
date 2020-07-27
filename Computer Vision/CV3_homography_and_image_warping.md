# Homography and Image Warping

## alignment vs. warping

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gh527ypeq1j31bq0u0dlt.jpg" style="zoom:33%;" />

## Mosaic

* reproject images onto a common plane
* form mosaic on this plane
* mosaic is a synthetic wide-angle camera

### image reprojection: homography

- a projective transform is a mapping between any two PPs with the same center of projection

  - Rectangle can map to arbitrary quadrilateral
  - parallel lines aren't preserved
  - but must preserve straight lines

- Homography:

- $$
  \begin{bmatrix} wx'\\wy' \\w\end{bmatrix}
  $$

- 

- 