# Mathematical methods and functions

## Table of Contents
## 1. Vectors
## 2. Trigonometric Functions
## 3. Graphing / Interpolation
## 4. Minimum / Maximum values

## 1. Vectors

Squirrel_re supports the use of mathematical / physics vectors without having to manually include a vector header / library in every script.

Vectors are declared with the "vector" keyword:

```
vector origin
vector point = <x,y,z> // vector quantities (magnitude / length, direction) in simulated 3D space, derived from x, y, z coordinates that represent offsets on each axis from the origin. These are Position Vectors, also known as Point Vectors. Read more about Position Vectors on Wikipedia

Hence, vectors are also used to store 3D coordinates and 3D rotation angles (pitch, yaw, roll).

vector someVector = <0, 2, 5> 
vector angleOffset = <0,0,0>
vector ornull clampedPos // vector declarations can use the ornull keyword

The inverse of a vector can be obtained with

-<x,y,z>

Naturally, vector coordinates can also be negative

<0, -90, 0>

Squirrel_re has a macro for the zero vector: ZERO_VECTOR // (<0,0,0>)

```

```
Vectors are also supported as a type of array element
array< vector > vectorArray
```

### Accessing the coordinates by which a vector is defined

The x,y,z coordinates are accessed using the dot (.) operator:

```
someVector.x
someVector.y
someVector.z
```

Strings can be converted to vectors using
```
StringToVector()
```

### Getting the distance between two points in 3D space

Method 1 (distance magnitude only):

```
float distance = Distance( vector point1, vector point2 ) 
```

Method 2 (distance magnitude and direction, full distance vector):

```
vector originA = entityA.GetOrigin()
vector originB = entityB.GetOrigin()
vector vecToEnt = ( originB - originA ) 
Note: this vector contains BOTH a magnitude and a direction!
If you only want the direction, you must normalize the vector with
Normalize(vector)
```


### Distance-related functions
```
Distance( vector pos1, vector pos2 )
ArrayEntityWithinDistance( array<entity>, vector originPos, float distance )
WaitUntilWithinDistance( entity player, entity titan, float dist )
WaitUntilBeyondDistance( entity player, entity titan, float dist )
```

### Entity vector methods

```
player.EyePosition()
player.GetViewVector() // only works on the player entity
owner.GetViewForward() // the owner is an entity
entity.GetAngles()
entity.SetAngles()
entity.GetOrigin()
entity.SetOrigin()
projectile.GetUpVector() // "projectile" here is still an entity
entity.GetAttachmentOrigin( attachID )
entity.GetAttachmentAngles( attachID )
vector reverseAngles = AnglesCompose( awall_base.GetAngles(), <0, 180, 0>) // Angular composition
entity.GetForwardVector()
entity.GetRightVector()
entity.GetLeftVector()
entity.GetWorldSpaceCenter()
entity.GetVelocity() // velocity =/= speed, velocity is a vector
vector lookDir = ent.GetSmoothedVelocity() // velocity =/= speed, velocity is a vector
```

### Vector Normalization
```
Normalize( vector ) // // sets vector length to 1, used for getting the direction of a vectors 

vector direction = Normalize(end - start)
```

### Vector operations

```
CrossProduct()

DotProduct()
```

### Other vector functions

```
GetRotationVector( entity rotator )
```
```
VectorToAngles() // gets rotation angles from a vector; funnily enough, those angles are also stored in a vector
```
```
AnglesToForward()
AnglesToUp()
AnglesToDown()
AnglesToLeft() // angle triads where 2 coordinates are 0 and the 3rd is a multiple of 90 degrees (90, -90, etc.)
AnglesToRight()
```
```
VectorReflectionAcrossNormal( vector vec, vector normal) // Gets the reflected vector from the incident ray by casting a normal vector (perpendicular to the surface), same as in the Optics branch of Physics
```

```
VectorRotateAxis()
```

```
array<vector> function ArrayFarthestVector(array<vector> vecArray, vector origin)  
ArrayDistanceResultsVector(vecArray, origin)  
array<vector> function ArrayClosestVectorWithinDistance(array <vector>, vecArray, vector origin, float maxDistance)  
array<vector> function ArrayClosest2DVector(array <vector> entArray, vector origin)  
```

### Bounding Box / Collision Mesh Vectors
```
vector MAP_MINS = volume.GetOrigin() + volume.GetBoundingMins()
vector MAP_MAXS = volume.GetOrigin() + volume.GetBoundingMaxs()
```

## 2. Trigonometric Functions

```
sin( float theta ) // Sin of angle theta
cos( float theta ) // Cos of angle theta
```


```
float arcLen = GetArcLengthDeg( intersectionAnglesDeg[ 0 ], intersectionAnglesDeg[ 1 ])
```


// tan, arctan? cotan?


## 3. Graphing / Interpolation 
```
Linear interpolation / Linear function graphing, a = mx + b, where a = f(x), the image of x, m = the slope, b = a constant
These functions are passed the coordinates of two points A(Xa, Ya), B(Xb, Yb), in order to be able to draw the graph of the function and extract the first degree equation of the linear function (solving m, b).
Graph(V,A,B,C,D) // where V = x, the current value passed to the function to be linearly interpolated / graphed, A = Xa (carthesian coordinate of point A on the oX axis), B = Xb (carthesian coordinate of point), C = Ya D = Yb

The result of the function is f(x), successfully interpolating / extrapolating point C(Xc, Yc) using the linear equation extracted from the coordinates of the two other points given (point A(Xa, Yb) and point B(Xb, Yb)).
IMPORTANTLY, the point C(Xc, Yc) is NOT clamped between points A and B, meaning it can be ANYWHERE on the graph of the linear equation!

GraphCapped(V,A,B,C, D) // same as Graph(), except the values are clamped to A and C, meaning f(x) is clamped between Ya and Yb, hence point C(Xc, Yb) will be clamped between point A(Xa, Ya) and point B(Xb, Yb) on the graph derived from the linear equation
```


### Bezier Curves

Apex Legends' codebase has a lot of functions for splines / Bezier curves

```
GetAllPointsOnBezier( array<vector> points, int numSegments, float debugDrawTime = 0.0)
GetSinglePointOnBezier( array <vector> points, float t)
GetBezierOfPath( array<vector> path, int numSegments, float debugTime = 0.0)
GetBezierOfPathLoop( array<vector> path, int numSegments)
GetBezierNodeTangent(vector nodePos, vector predecessorPos, vector successorPos)
```

## 4. Minimum, Maximum Values

```
abs() // gets the absolute value of a number (no sign)

absmin()
absmax()

min()
max()
```

