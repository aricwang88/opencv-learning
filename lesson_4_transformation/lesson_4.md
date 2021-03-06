# Transformation
Operations like translation, rotation, affine transformation etc.

## Scaling
Resize

```
img = cv2.imread(image_path)
resized = cv2.resize(img, None, fx=1.5, fy=1.5, interpolation=cv2.INTER_LINEAR)
```
interpolation:

cv2.INTER_AREA: shrinking,

cv2.INTER_CUBIC: zooming, SLOW,

cv2.INTER_LINEAR: zooming

Wondered that the three methods can all do shrinking or scaling, while
using as the documentation suggests would produce a better result in generally.

## Translation
Shifting of object location.

```
img = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
rows, cols = img.shape

M = np.float32([[1, 0, 100], [0, 1, 50]])

res = cv2.warpAffine(img, M, (cols, rows))
```

If you know the shift in (x,y) direction, let it be (tx,ty), you can
create the transformation matrix M as follows: M=`[[1 0 x], [0 1 y]]`

As the code above shows that the pixels in `img` will all be move 100 pixels
in `x` direction, move 50 pixels in `y` direction

**warning**
> Third argument of the cv.warpAffine() function is the size of the output image,
> which should be in the form of **(width, height)**.
> Remember width = number of columns, and height = number of rows.

## Rotation
Rotate image by an angle θ
```
M = cv2.getRotationMatrix2D((cols / 2, rows / 2), 60, 0.5)
res = cv2.warpAffine(img, M, (cols, rows))
```
- Get rotation matrix: cv2.getRotationMatrix2D
  - Take three arguments: 1st is the center point which to be the anchor
    2nd is θ the angle in degree, 3rd is scalar
- Rotate with cv2.warpAffine() function

## Affine Transformation

Transform by `three points`, the points in output image is really the
points in input image, this (row, column) transformation of these three
points generate a matrix, with which can transform all the points in the
input image to the points in the output image

```
img = cv2.imread(image_path)
rows, cols, channel = img.shape

pts_src = np.float32([[50, 50], [200, 50], [50, 200]])
pts_dst = np.float32([[10, 100], [200, 80], [100, 650]])

M = cv2.getAffineTransform(pts_src, pts_dst)
res = cv2.warpAffine(img, M, (cols, rows))
```

## Perspective Transformation

Like Affine Transformation, Perspective Transformation depends on the
Matrix that generated by the points transformation from input image to
output image, besides which `3` points needed in Affine Transformation,
but `4` points in Perspective Transformation
```
img = cv2.imread(image_path)
rows, cols, channel = img.shape
pts1 = np.float32([[56, 65], [368, 52], [28, 387], [389, 390]])
pts2 = np.float32([[0, 0], [300, 0], [0, 300], [300, 300]])
M = cv2.getPerspectiveTransform(pts1, pts2)
res = cv2.warpPerspective(img, M, (cols, rows))
```
