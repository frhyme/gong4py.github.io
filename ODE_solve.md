# Solving Differential Equation

## Taekho You
## 2017.04.10

---
# Objective

- Mathematica와 같은 문제 풀이 방법이 있는지 확인
- 있으면 ODE의 Analytic solution을 구해봄
$$\dot \bold x = f(\bold x) $$
- 예:
$$\dot x = ax$$
$$x = x_0e^{ax}$$
---
# Solving Equation

- ODE를 푸는 방법을 제공하는 library
  - Scipy, PyDS
- Analytical solution을 제공하는 library
  - Sympy?
---
# Functions in Scipy
- Intergators of ODE systems in ___scipy.integrate___
```python
# Integrate a system of ordinary differential equations
Odient(func, y0, t[,args,Dfun,col_deriv,...]

# A generic interface class to numeric integrators
ode(f[,jac])

# A wrapper of ode for complex systems
complex_ode(f[,jac])

# Solve a boundary-value problem for a system of ODEs.
solve_bvp(fun,bc,x,y[,p,S,fun_jac,...])
```

---
# Solving numerically with Scipy: odeint
$$\ddot\theta(t) + b\dot\theta(t) + c\sin(\theta(t)) = 0$$ 
&rarr;
$$\dot\theta(t) = \omega(t) $$
$$\dot\omega(t) = -b \omega(t) - c\sin(\theta(t))$$

```python
from scipy.integrate import odient
def pend(y,t,b,c):
	theta, omega = y
    dydt = [omega, -b*omega - c*np.sin(theta)]
    return dydt
```
---
# Solving numerically with Scipy: odeint
```python
# initial condition
b = 0.25, c= 5.0, y0 = [np.pi - 0.1, 0.0]

# set interval of t as 0.1
t = np.linspace(0,10,101)

# get integrating solution with odient
sol = odient(pend, y0, t, args=(b,c))
```

![center size=60%](/Users/TaekhoYou/Desktop/AnalySol/python_scipy_integrate_ODE.png)

---
# Solving numerically with Scipy: ode
```python
from scipy.integrate import ode
def f(t,y,arg1):
	return [1j*arg1*y[0] + y[1], -arg1*y[1]**2]
def jac(t,y,arg1):
	return [[1j*arg1,1],[0,-arg1*2*y[1]]]

y0, t0 = [1.0j,2.0],0

r = ode(f,jac).set_integrator('zvode',method='bdf')
r.set_initial_value(y0,t0).set_f_params(2.0).set_jac_params(2.0)
t1 = 10
dt = 1
while r.successful() and r.t < t1:
	print(r.t+dgt, r.integrate(r.t+dt))
```
---
# Difference between odeint and ode
- Integration method
  - odeint: Isoda라는 FORTRAN library에서 제공하는 integration 방법을 사용함 (real value)
  - ode: vode, zvode, Isoda, dopri5, dop853 등의 여러 방법을 사용 가능 (real value + complex value)
- Jacobian의 유무
  - jacobian을 활용하면 정교한 방법들을 사용 가능함

---
# Get Analytical Solution with Sympy
- dsolve를 이용해서 해를 구할 수 있는 것으로 보임
- 안의 알고리즘은 어떻게 되는건지 전혀 모르겠음
- 기본 형태
$$\ddot f(x) + 9f(x) = 0$$
```python
from sympy import Function, dsolve, Eq, Derivative, sin, cos, symbols
from sympy.abc import x
f = Function('f')
dsolve(Derivative(f(x),x,x) + 9*f(x), f(x))
```
```python
Eq(f(x),C1*sin(3*x) + C2*cos(3*x))
```
---
# Get Analytica Solution with Sympy
- 처음 문제를 적기 위해서 함수 형태로 정의해야 함
- x와 같은 변수도 정의가 필요
```python
# define function
f = Function('f')
f = Function('f')(x)
f = symbols('f', function = True)

# define of x
from sympy.abc import x
x = sylbols('x')
```

---
# Get Analytica Solution with Sympy
- 식을 적은 후 dsolve를 이용해서 풀이해야 함
- 풀 때 hint를 이용해 여러 종류를 지정할 수 있음
- 답이 나온 후 표현을 예쁘게 하기 위해 pprint를 사용
```python
eq = (Eq(Derivative(x(t),t), 12*t*x(t)+ 8*y(t))
,Eq(Derivative(y(t),t),12*x(t) + 7*t*y(t)))
dsolve(eq)
dsolve(eq, hint='almost_linear')
dsolve(eq, hint='1st_exact')
```