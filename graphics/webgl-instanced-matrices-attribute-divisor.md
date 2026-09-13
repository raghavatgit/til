# WebGL Vertex Attribute Divisor for Instanced Geometry

*Date: 2026-09-13*  
*Category: Graphics & WebGL*

## Overview

When rendering thousands of dynamic entities (e.g. dust motes, foliage, or spatial UI nodes), standard WebGL attributes advance once per vertex. Using `vertexAttribDivisor` (available natively in WebGL 2.0 or via `ANGLE_instanced_arrays` in WebGL 1.0), attributes can be configured to advance once per **instance** instead.

## Mechanism

```javascript
// Attribute index for instance transform matrix or position
const INSTANCE_OFFSET_LOC = 3;

// Configure attribute buffer
gl.bindBuffer(gl.ARRAY_BUFFER, instanceOffsetBuffer);
gl.enableVertexAttribArray(INSTANCE_OFFSET_LOC);
gl.vertexAttribPointer(INSTANCE_OFFSET_LOC, 3, gl.FLOAT, false, 0, 0);

// Set divisor: 0 = advance per vertex, 1 = advance once per instance
gl.vertexAttribDivisor(INSTANCE_OFFSET_LOC, 1);
```

## Vertex Shader Usage

```glsl
#version 300 es
layout(location = 0) in vec3 aVertexPosition; // Divisor 0: per-vertex
layout(location = 3) in vec3 aInstanceOffset;  // Divisor 1: per-instance

uniform mat4 uProjectionMatrix;
uniform mat4 uViewMatrix;

void main() {
    vec3 worldPosition = aVertexPosition + aInstanceOffset;
    gl_Position = uProjectionMatrix * uViewMatrix * vec4(worldPosition, 1.0);
}
```

## Key Benefit
Enables the GPU to render tens of thousands of individual objects in a single `gl.drawArraysInstanced()` or `gl.drawElementsInstanced()` call, completely eliminating JavaScript CPU draw loop overhead.
