# Kalman Filtering: Equations and Python example

## Introduction

Kalman filtering is a recursive mathematical algorithm used for estimating the state of a dynamic system based on measurements from sensors and predictions from a dynamic model. It is widely used in various fields, including autonomous localization in vehicles. The Kalman filter combines the principles of state estimation and covariance analysis to provide an optimal solution in terms of minimizing estimation error. In this technical explanation, we will delve into the underlying equations and demonstrate the implementation of Kalman filtering using Python code snippets.  

1. Prediction Step:  
   In the prediction step, the Kalman filter predicts the current state based on the previous state estimate and the system's dynamic model. The prediction equations are as follows:  

State Prediction:  

$$
x̂ₖ = F * x̂ₖ₋₁ + B * uₖ
$$

Covariance Prediction:  

$$
Pₖ = F * Pₖ₋₁ * Fᵀ + Q
$$

Where:  

- $x̂ₖ$ is the predicted state at time $k$  

- $F$ is the state transition matrix that describes the dynamics of the system  

- $B$ is the control input matrix  

- $uₖ$ is the control input at time $k$  

- $Pₖ$ is the predicted error covariance matrix at time $k$  

- $Q$ is the process noise covariance matrix  
2. Update Step:  
   In the update step, the Kalman filter incorporates sensor measurements to correct the predicted state and update the estimation. The update equations are as follows:  

Kalman Gain:  

$$
Kₖ = Pₖ * Hᵀ * (H * Pₖ * Hᵀ + R)⁻¹
$$

State Update:  

$$
x̂ₖ = x̂ₖ + Kₖ * (zₖ - H * x̂ₖ)
$$

Covariance Update:  

$$
Pₖ = (I - Kₖ * H) * Pₖ
$$

Where:  

- $Kₖ$ is the Kalman gain at time $k$  
- $H$ is the measurement matrix that maps the state to the measurement space  
- $zₖ$ is the measurement at time $k$  
- $R$ is the measurement noise covariance matrix  
- $I$ is the identity matrix  

## Python Code Snippets

Here's an example implementation of the Kalman filter in Python, assuming a 1D system:  

```python
import numpy as np  

# Initialization  
x_hat = np.array([[0]])  # Initial state estimate  
P = np.array([[1]])      # Initial error covariance matrix  
F = np.array([[1]])      # State transition matrix  
Q = np.array([[0.1]])    # Process noise covariance matrix  
H = np.array([[1]])      # Measurement matrix  
R = np.array([[0.5]])    # Measurement noise covariance matrix  

# Prediction step  
def predict(x_hat, P):  
    x_hat = F.dot(x_hat)  
    P = F.dot(P).dot(F.T) + Q  
    return x_hat, P  

# Update step  
def update(x_hat, P, z):  
    K = P.dot(H.T).dot(np.linalg.inv(H.dot(P).dot(H.T) + R))  
    x_hat = x_hat + K.dot(z - H.dot(x_hat))  
    P = (np.eye(len(x_hat)) - K.dot(H)).dot(P)  
    return x_hat, P  

# Example usage  
measurements = [1.2, 1.7, 2.1, 2.8]  # Example measurements  

for z in measurements:  
    # Prediction step  
    x_hat, P = predict(x_hat, P)  

    # Update step  
    x_hat, P = update(x_hat, P, z)  

    print("Estimated state:", x_hat.flatten()[0])  
```

This Python code snippet demonstrates a basic implementation of the Kalman filter for a 1D system. The `predict` function performs the prediction step, while the `update` function carries out the update step using sensor measurements. The estimated state is updated at each iteration based on the predicted state and the measurement. The code can be adapted and extended for multidimensional systems and different types of sensor measurements.  

## Conclusion

Kalman filtering is a powerful technique for estimating the state of dynamic systems, including autonomous localization in vehicles. By incorporating sensor measurements and predictions from a dynamic model, the Kalman filter provides an optimal estimation while accounting for measurement noise and uncertainties. The Python code snippets presented here offer a starting point for implementing the Kalman filter in practical applications, enabling accurate state estimation and enhancing the capabilities of autonomous systems.

July 10<sup>th</sup>, 2023
