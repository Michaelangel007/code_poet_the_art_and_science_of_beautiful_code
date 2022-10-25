# Code Poet: The Art and Science of Beautiful Code

# What is beautiful code?
# Why is it important?
# Examples

## OpenGL Perspective Frustum Matrix

Khronos has this [GluPerspective()](https://www.khronos.org/opengl/wiki/GluPerspective_code) code:

```c
void glhFrustumf2(float *matrix, float left, float right, float bottom, float top,
                  float znear, float zfar)
{
    float temp, temp2, temp3, temp4;
    temp = 2.0 * znear;
    temp2 = right - left;
    temp3 = top - bottom;
    temp4 = zfar - znear;
    matrix[0] = temp / temp2;
    matrix[1] = 0.0;
    matrix[2] = 0.0;
    matrix[3] = 0.0;
    matrix[4] = 0.0;
    matrix[5] = temp / temp3;
    matrix[6] = 0.0;
    matrix[7] = 0.0;
    matrix[8] = (right + left) / temp2;
    matrix[9] = (top + bottom) / temp3;
    matrix[10] = (-zfar - znear) / temp4;
    matrix[11] = -1.0;
    matrix[12] = 0.0;
    matrix[13] = 0.0;
    matrix[14] = (-temp * zfar) / temp4;
    matrix[15] = 0.0;
}
```

This is garbage code due to bad naming and bad formatting:

* WTF does temp1 do?
* WTF does temp2 do?
* WTF doees temp3 do?
* WTF does temp4 do?

Compare and contrast with this cleaner code:

```c
// ======================================================================== 
void openglFrustum( float mProjection[16], 
    const float nLeft  , const float nRight, 
    const float nBottom, const float nTop  ,
    const float nNear  , const float nFar  )
{
    /* */ float *m = &mProjection[0] ;
    const float dx = nRight - nLeft  ; // difference
    const float dy = nTop   - nBottom;
    const float dz = nFar   - nNear  ;
    const float sx = nRight + nLeft  ; // sum
    const float sy = nTop   + nBottom;
    const float sz = nFar   + nNear  ;
    const float n2 = 2.f    * nNear  ; // common factor: 2 * zNear

    m[0] = n2 / dx; m[4] = 0.f    ; m[ 8] =  sx / dx; m[12] = 0.f            ;
    m[1] = 0.f    ; m[5] = n2 / dy; m[ 9] =  sy / dy; m[13] = 0.f            ;
    m[2] = 0.f    ; m[6] = 0.f    ; m[10] = -sz / dz; m[14] = n2 * -nFar / dz;
    m[3] = 0.f    ; m[7] = 0.f    ; m[11] = -1.f    ; m[15] = 0.f            ;
}
```
