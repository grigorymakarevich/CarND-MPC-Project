# Model Predictive Control (MPC)

## The model

I was using the kinematic model described in Udacity lectures:

The state consists of vehicles corrdinates - px, py, orientation - psi,
velocity - v, and two error measurements - cross-track error (cte), and psi-error (epsi).
Actuators are acceleration and steering angle. The mode uses the following
update equations:

x[t+1] = x[t] + v[t] * cos(psi[t]) * dt

y[t+1] = y[t] + v[t] * sin(psi[t]) * dt

psi[t+1] = psi[t] + v[t] / Lf * delta[t] * dt

v[t+1] = v[t] + a[t] * dt

cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt

epsi[t+1] = psi[t] - psides[t] + v[t] * delta[t] / Lf * dt


## Timestep Length and Elapsed Duration (N & dt)

I considered the following while chosing the values for N and dt:

- The length of predicted track should approximately match the length of the given
  trajectory at every moment. Considering varying velocity, that is difficult to maintain
  all through the track, however, it should match for most time points

- Too small value of dt will tend to over-optimize for tiny initial period
  Too large value of dt will tend to under-optimize for initial steps

So, I have played with different values for dt ranging from 0.001 to 1. For each of dt
I tried multiple values of N trying to match the length of trajectory.
Most cases demonstrated very chaotic behavior early on. I have ended up with values N = 15; dt = 0.12

## Polynomial Fitting and MPC Preprocessing

My initial attempts to predict using track coordinates were not successful. So, I have tried to transform
the coordinates from track to car coordinate system, i.e. before fitting the data, the coordinates are transformed
into car coordinates.

Then coordinates are fitted using polynom of 3 degree.

## Model Predictive Control with Latency

I have proceeded wit hthe approach indicated by the reviwer as one of the suggestions:
"to simulate latency at every timestep, instead of taking delta0 and a0 from last timestep,
 you take the values from another timestep before, with the disadvantage that used dt value should be the same as latency."
 I have proceeded with this approach, and it worked.