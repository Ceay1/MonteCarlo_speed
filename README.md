# Summary

Monte Carlo method is a standard tool to solve models. The method, however, is very slow compared with other possible alternatives.

The Python example below compares speed of two methods that calculate the pi number: (1) Monte Carlo and (2) numerical integration. Both methods calculate the area of one quadrant of the unitary circle and multiply the result by 4 in order to arrive at the pi number.

With 1 million simulations, a typical error of the Monte Carlo method is e-3 (the number is random and depends on the seed), while numerical integration has an error of e-15. Monte Carlo method takes 1.2 seconds, while numerical integration finishes in 0.0001 seconds. The numerical integration method is so efficient that all the computing power in the world, if used for Monte Carlo simulation, would not match the speed of the numerical integration method to achieve the same precision.

To achieve the same precision, one needs to increase the number of simulations by the squared ratio of the precisions, because precision of Monte Carlo improves as the square root of the numer of simulations.

# Python script

```Python
from scipy import integrate 
from timeit import default_timer  as timer
from random import uniform, seed
from math import pi, sqrt

# Random seed
seed(20)
# Number of simulations for Monte Carlo
N=1_000_000

print("\nPi: ", pi,"\n")

# **** Monte Carlo ****

start=timer()
count=0
for _ in range(N): count += ((uniform(0,1)**2 + uniform(0,1)**2)<=1)
pi_MC=count/N*4
time_MC=timer()-start
precision_MC_obs=abs(pi_MC-pi)
# 3 sigma of theoretical standard deviation
precision_MC_empir=3*4*sqrt((pi/4)*(1-(pi/4)))/sqrt(N)

# **** Monte Carlo output ****

print("*** Monte Carlo ***\n")
print("Number of simulations: {:,}".format(N))
print("Estimate of pi:", pi_MC)
print("Observed error:", precision_MC_obs)
print("Theoretical error for 99.7% confidence (3 std):", precision_MC_empir)
print("Time: {:0.5f} seconds".format(time_MC))

# **** Numerical integration ****

start=timer()
pi_int=integrate.quad(lambda x: sqrt(1-x**2), 0, 1)[0]*4
precision_int=round(abs(pi_int-pi) if abs(pi_int-pi)>0 else 0.000000000000001, 15)
time_int=timer()-start

# **** Numerical integration output ****

print("\n*** Numerical integration ***\n")
print("Estimate of pi:", pi_int)
print("Observed error:", precision_int)
print("Time: {:0.5f} seconds".format(time_int))


print("\nYears to achieve numerical integration precision through Monte Carlo simulations: {:,}".\
      format(int(pow(precision_MC_obs/precision_int,2)*time_MC/(3600*24*365.25))))
```

# Python output

```
Pi:  3.141592653589793 

*** Monte Carlo ***

Number of simulations: 1,000,000
Estimate of pi: 3.142616
Observed error: 0.001023346410206738
Theoretical error for 99.7% confidence (3 std): 0.004926550103208972
Time: 1.17200 seconds

*** Numerical integration ***

Estimate of pi: 3.141592653589792
Observed error: 1e-15
Time: 0.00012 seconds

Years to achieve numerical integration precision through Monte Carlo simulations: 38,892,640,486,620,824

```
