# Tutorial 3 Solutions
## Task 1
`vector.sty`
```typescript
/**************DO NOT TOUCH ZONE - START**************/
/* here are some useful constants that we use to draw
 * the vector space 
*/
canvas {
  width = 800
  height = 700
}

const { 
  scalar vectorSpaceSize = 350.0
  scalar arrowheadSize = 0.7
  scalar lineThickness = 1.
  scalar arrowThickness = 1.5
  color gray = rgba(0.6, 0.6, 0.6, 1.)
  color lightBlue = rgba(0.2, 0.4, 0.8, 1.0)
  color lightGray = rgba(252, 252, 252, 0.015)
  color none = rgba(0., 0., 0., 0.)
}

/* here we draw a vector space by defining an origin
 * of the vector space, and x-axis, y-axis that are 
 * centered at the origin 
 */
forall VectorSpace U { 
    scalar axisSize = const.vectorSpaceSize / 2.0 
    vec2 U.origin = (0., 0.)
    vec2 o = U.origin /* just so we don't need to type U.origin everytime */
    U.axisColor = const.gray

    U.background = Square {
        center : U.origin
        side : const.vectorSpaceSize
        color : const.lightGray
        strokeColor : const.none
    }

    U.xAxis = Line {
        start : (o[0] - axisSize, o[1]) 
        end : (o[0] + axisSize, o[1])
        thickness : const.lineThickness
        style : "solid"
        color : U.axisColor
        leftArrowhead: true
        rightArrowhead: true
        arrowheadSize : const.arrowheadSize * 2.
    }

    U.yAxis = Line {
           start : (o[0], o[1] - axisSize)
             end : (o[0], o[1] + axisSize)
       thickness : const.lineThickness
           style : "solid"
           color : U.axisColor
           leftArrowhead: true
           rightArrowhead: true
        arrowheadSize : const.arrowheadSize * 2.
    }

    U.text = Text {
        string : U.label
        center : (U.origin[0] - axisSize, U.origin[1] + axisSize)
        color : U.axisColor
    }
}
/**************DO NOT TOUCH ZONE - END**************/

/**************YOUR CODE - START********************/
forall Vector u; VectorSpace U
where In(u,U) {
  u.vector = (?, ?)

  u.shape = Arrow {
    start: U.origin
    end : U.origin + u.vector
    thickness : 3.0
    color : const.lightBlue
    arrowheadSize : const.arrowheadSize
  }

  u.text = Text {
    string : u.label
    color : u.shape.color
  }

  ensure contains(U.background, u.shape)
  ensure contains(U.background, u.text)
  ensure atDist(u.shape, u.text, 15.0)
  ensure minSize(u.shape)

  layer u.text above U.xAxis
  layer u.text above U.yAxis
}
/**************YOUR CODE - END**********************/
```
## Task 2

`linearAlgebra.dsl`
```typescript
type VectorSpace
type Vector
type Scalar
predicate In: Vector * VectorSpace V
function addV: Vector * Vector -> Vector
```

`vector.sub`
```typescript
VectorSpace U
Vector v 
Vector w
In(v, U)
In(w, U)
Vector u := addV(v, w)
In(u, U)
AutoLabel All
```

`vector.sty`
```typescript
/* ... same as starter code */
/**************DO NOT TOUCH ZONE - END**************/

/**************YOUR CODE - START********************/
forall Vector u; VectorSpace U
where In(u,U) {
  u.text = Text {
    string : u.label
    color : u.shape.color
  }
  
  u.shape = Arrow {
    start: U.origin
    end : U.origin + u.vector
    thickness : 3.0
    color : const.lightBlue
    arrowheadSize : const.arrowheadSize
  }

  ensure contains(U.background, u.shape)
  ensure contains(U.background, u.text)
  ensure atDist(u.shape, u.text, 15.0)
  ensure minSize(u.shape)

  layer u.text above U.xAxis
  layer u.text above U.yAxis
}

-- Tutorial Part 2 Changes:
forall Vector u; Vector v; Vector w; VectorSpace U
where u := addV(v,w); In(u, U); In(v, U); In(w, U) {
  override u.shape.end = v.shape.end + w.shape.end - U.origin
}
/**************YOUR CODE - END**********************************/
```

## Exercise 1
`linearAlgebra.dsl`
```typescript
type VectorSpace
type Vector
type Scalar
predicate In: Vector * VectorSpace V
function subV: Vector * Vector -> Vector
```

`vector.sub`
```typescript
VectorSpace U
Vector v 
Vector w
In(v, U)
In(w, U)
Vector u := subV(v, w)
In(u, U)
AutoLabel All
```

`vector.sty`
```typescript
/**************YOUR CODE - START********************/
forall Vector u; VectorSpace U
where In(u,U) {
 /* ...concatenated, this is the same as Tutorial Part 1 */
}

-- Exercise 1 Changes:
forall Vector u; Vector v; Vector w; VectorSpace U
where u := subV(v,w); In(u, U); In(v, U); In(w, U){
  override u.shape.end = v.shape.end - w.shape.end - U.origin
}
/**************YOUR CODE - END**********************************/
```

## Exercise 2
`linearAlgebra.dsl`
```typescript
type VectorSpace
type Vector
type Scalar
predicate In: Vector * VectorSpace V
function scalarMult: Scalar * Vector -> Vector
```

`vector.sub`
```typescript
VectorSpace U
Vector v 
In(v, U)
Scalar a;
Vector u := scalarMult(a, v)
In(u, U)
AutoLabel All
```

`vector.sty`
```typescript
/**************YOUR CODE - START********************/
forall Vector u; VectorSpace U
where In(u,U) {
 /* ...concatenated, this is the same as Tutorial Part 1 */
}

-- Exercise 2 Changes:
forall Scalar a {
  -- a.scalar = 5. /* example of setting a fixed value scalar, note that "--" indicates an inline comment */
  a.scalar = ?  /* randomized value decided by Penrose */
  ensure inRange(a.scalar, 2., 5.);
}

forall Scalar a; Vector u; Vector v; VectorSpace U
where u := scalarMult(a, v); In(u, U); In(v, U){
   override u.shape.end = (a.scalar * v.shape.end) - U.origin 
}
/**************YOUR CODE - END**********************************/
```
## Exercise 3
`linearAlgebra.dsl`
```typescript
/* This is the same as Tutorial Part 2 */
type VectorSpace
type Vector
type Scalar
predicate In: Vector * VectorSpace V
function addV: Vector * Vector -> Vector
```

`vector.sub`
```typescript
/* This is the same as Tutorial Part 2 */
VectorSpace U
Vector v 
Vector w
In(v, U)
In(w, U)
Vector u := addV(v, w)
In(u, U)
AutoLabel All
```

`vector.sty`
```typescript
where In(u,U) {
 /* ...concatenated, this is the same as Tutorial Part 1 */
}

forall Vector u; Vector v; Vector w; VectorSpace U
where u := addV(v,w); In(u, U); In(v, U); In(w, U) {
  override u.shape.end = v.shape.end + w.shape.end - U.origin

  /************Exercise 3: Parallelogram(start)*****************/
  u.dashed_v = Arrow {
    start: (w.shape.end[0], w.shape.end[1])
    end: (u.shape.end[0], u.shape.end[1])
    thickness : const.arrowThickness
    style : "dashed"
    arrowheadSize : const.arrowheadSize
  }

  u.dashed_w = Arrow {
    start: (v.shape.end[0], v.shape.end[1])
    end: (u.shape.end[0], u.shape.end[1])
    thickness : const.arrowThickness
    style : "dashed"
    arrowheadSize : const.arrowheadSize
  }

  u.dashed_w below u.shape
  u.dashed_v below u.shape
  /************Exercise 3: Parallelogram(end)*******************/
}
```
