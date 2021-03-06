# Backward functions for Linear Algebra

[![Build Status](https://travis-ci.com/GiggleLiu/LinalgBackwards.jl.svg?branch=master)](https://travis-ci.com/GiggleLiu/LinalgBackwards.jl)
[![Codecov](https://codecov.io/gh/GiggleLiu/LinalgBackwards.jl/branch/master/graph/badge.svg)](https://codecov.io/gh/GiggleLiu/LinalgBackwards.jl)

![#f03c15](https://placehold.it/15/f03c15/000000?text=+) This project is still in progress ...

Backward functions for linear algebras,
It is currently ported to `Flux.jl` for testing, but these porting codes will be moved to other places (like merging them to `Flux.jl`) in the future.

## Why we need LinearBackwards.jl?
Not only in Julia, but also in well known machine learning packages in python like pytorch, one can hardly find a numerical stable implementations of linear algebra function. This missing piece is crutial to autodiff applications in tensor networks algorithms.

## Table of Supported Functions

We put `_` before functions since we changed the rule of functions in `LinearAlgebra`, the outputs are Tuples.

- [x] _svd
- [x] qr
- [ ] _cholesky   # Nabla.jl
- [ ] _powermethod   # we need fixed point methods, trying hard ...
- [x] _eigen      # linear BP paper
- [x] _lq         # similar to qr
- [ ] pfaffian    # find it nowhere, lol

For `logdet`, `det` and `tr`, people can find it in `ChainRules.jl` and `Nabla.jl`.

Derivation of adjoint backward functions could be found [here](https://giggleliu.github.io/2019/04/02/einsumbp.html).

## How to Use
It currently ports into `Zygote.jl`
```julia
using Zygote, LinalgBackwards

function loss(A)
    M, N = size(A)
    U, S, V = svd(A)
    psi = U[:,1]
    H = randn(ComplexF64, M, M)
    H+=H'
    real(psi'*H*psi)[]
end

a = randn(ComplexF64, 4, 6)
g = loss'(a)
```

Try something interesting (the backward of TRG code, `TensorOperations.jl` (as well as patch https://github.com/Jutho/TensorOperations.jl/pull/59) is required.)
```bash
julia test/trg.py
```
