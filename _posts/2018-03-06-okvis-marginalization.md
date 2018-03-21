---
layout: cnmath
title: "OKVIS 笔记：边缘化实现"
date: 2018-03-06 01:00:00
categories: cn
tags: robotics VIO SLAM OKVIS
---

__目录__

* content
{:toc}

主要涉及源文件：

```
okvis_ceres/include/okvis/ceres/MarginalizationError.hpp
okvis_ceres/src/MarginalizationError.cpp
okvis_ceres/src/Estimator.cpp
```

### 原理

### 边缘化策略

### 封装结构

OKVIS 的边缘化在 `MarginalizationError` 类中实现，在 `Estimator` 类的 `applyMarginalizationStrategy()` 函数中调用。

先看看 `MarginalizationError` 内部封装数据：

```cpp
/// @name The internal storage of the linearised system.
/// lhs and rhs: (left hand side / right hand side)
/// H_*delta_Chi = _b - H_*Delta_Chi .
/// the lhs Hessian matrix is decomposed as _H = J^T*J = _U*S*_U^T ,
/// the rhs is decomposed as _b - _H*Delta_Chi = -J^T * (-pinv(J^T) * _b + J*Delta_Chi) ,
/// i.e. we have the ceres standard form with weighted Jacobians _J,
/// an identity information matrix, and an error
/// _e = -pinv(J^T) * _b + J*Delta_Chi .
/// _e = _e0 + J*Delta_Chi .
/// @{
Eigen::MatrixXd H_;  ///< lhs - Hessian
Eigen::VectorXd b0_;  ///<  rhs constant part
Eigen::VectorXd e0_;  ///<  _e0 := pinv(J^T) * _b0
Eigen::MatrixXd J_;  ///<  Jacobian such that _J^T * J == _H
Eigen::MatrixXd U_;  ///<  H_ = _U*_S*_U^T lhs Eigen decomposition
Eigen::VectorXd S_;  ///<  singular values
Eigen::VectorXd S_sqrt_;  ///<  cwise sqrt of _S, i.e. _S_sqrt*_S_sqrt=_S; _J=_U^T*_S_sqrt
Eigen::VectorXd S_pinv_;  ///<  pseudo inverse of _S
Eigen::VectorXd S_pinv_sqrt_;  ///<  cwise sqrt of _S_pinv, i.e. pinv(J^T)=_U^T*_S_pinv_sqrt
```

根据注释，大概思路如下。首先，考虑边缘化系统如下：

$$
\rm H \delta \chi = b - H \Delta\chi
$$

其中的 H 矩阵可以分解为 $\rm H=J^TJ=USU^T$，其中的 $\rm S$ 为奇异值矩阵。于是上式可以变换为

$$
\rm J^TJ \delta \chi = -J^T(-J^{T+}b+J \Delta\chi)
$$

此处 $()^+$ 为伪逆。于是，我们得到一个 Jacobian 为 $\rm J$，信息矩阵为单位阵，error 为 $\rm e=-J^{T+}b+J \Delta\chi$ 的最小二乘标准形式，可由 Ceres 来处理。


### 主要接口和函数

- `MarginalizationError::addResidualBlock()`

  把一个 Residual 及其对应的 ParameterBlock 加入现有的 Marginalization 系统中，同时将其从 Map 中剔除。此后 Marginalization 系统会一直维持该 Residual 在加入 Marginalization 系统时的线性点（即 FEJ），直至其被重新线性化或从当前窗口中剔除。

- `MarginalizationError::marginalizeOut()`




### Temp log

真的很烦，一旦真撸起这种工程逻辑，okvis 作者也就顾不上 OOP 了 :)

---

- `removeFrames`: states to remove (frame id, or oldest keyframe id)
- `removeAllButPose`: all in marg window (frame id)
- `allLinearizeFrames`: all in marg window (frame id)

---

_For those in_ `removeAllButPose`:

- Put SpeedAndBias states to `parameterBlockToBeMarg`.
- Add residuals concerning SpeedAndBias states to `marginalizationErrorPtr`. They are never ReprojectionError.

_end For_

---

_For those in_ `removeFrames`:

- Put Pose states to `parameterBlockToBeMarg`.
- Get residuals concerning Pose states. 
  - If a residual is PoseError, remove it from `mapPtr`;
  - If not, add it to `marginalizationErrorPtr` -- in this case, it must be a ReprojectionError.

- Put Extrinsics states to `parameterBlockToBeMarg`.
- Add residuals concerning Extrinsics states to `marginalizationErrorPtr`. They are always ReprojectionError.

_end For_

---

_For those in_ `landmarksMap_`:
- Get residuals concerning the landmark

  _For every residual_:
  - must be a ReprojectionError.
  - if the concerning Frame is in `removeFrames`, the landmark should not be skipped

  _end For_

  _If_ this landmark can be skipped, skip.

  _For every residual_:
  - get the concerning Frame

    _If_ 
    1. the concerning Frame is in `removeFrames` _And_ this landmark is observed by some Frame newer than the marg window, _Or_
    2. the concerning Frame is not in `allLinearizeFrames` _And_ this landmark is not observed by some Frame newer than the marg window:

    _then_
 
    - remove the residual only

    _Else if_ 
    1. the concerning Frame is in `allLinearizeFrames` _And_ this landmark is not observed by some Frame newer than the marg window
 
    _then_
 
    - remove the residual only




