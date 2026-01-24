# A Unified Physical Model of Feedback Stability in Control Theory

*Author: Percival Segui*\
*Prepared as an independent technical reference*.

*Engineering notes and a conceptual synthesis of a working control model.*


## Scope

This document presents a consolidated mental model of feedback stability in linear control systems.

The model integrates state-space dynamics, eigenvalues, transfer functions, frequency response, and Nyquist analysis into a single, internally consistent framework constrained by standard control theory.

---

## Structure of the Discussion

The development proceeds through the following progression:

* system dynamics are introduced in state-space form to establish the meaning of modes and exponential behavior
* eigenvalues are used to connect those modes to pole locations in the complex plane
* open-loop instability is examined in terms of right-half-plane poles
* feedback is introduced as a mechanism that reshapes effective system dynamics without altering plant physics
* the closed-loop characteristic equation is identified as the object governing stability
* Nyquist analysis is developed as a mapping of that characteristic equation over the right-half-plane
* encirclement is interpreted as evidence for the existence of unstable closed-loop modes

This progression is used consistently to relate time-domain behavior, frequency-domain response, and complex-plane analysis within a single causal framework.

---

## The Problem

Start with an inherently unstable plant, i.e., an inverted pendulum. An inverted pendulum will always have a RHP pole unless we add external control to stabilize it. So, this is a useful example.

We can write its behavior as a second-order ordinary differential equation (ODE), and by introducing state variables (i.e., position and velocity), we can convert that ODE into a first-order system of equations:

$$
\dot{x}(t) = A x(t) + B u(t)
$$

Here:

* $x(t)$ is the state vector (i.e., position and velocity),
* $u(t)$ is the input,
* $A$ is the state matrix that describes internal dynamics,
* $B$ maps inputs to state derivatives.

When we're looking at open-loop behavior, set $u(t) = 0$ to focus on natural system dynamics:

$$
\dot{x}(t) = A x(t)
$$

---

### The State Matrix

The matrix $A$ determines how the current state affects the *rate* of change of the state.

* It encodes the system’s internal feedback.
* It tells us whether the system tends to grow, decay, or oscillate on its own.

Let’s look for solutions of the form:

$$
x(t) = v e^{\lambda t}
$$

$v$ is a constant vector, a candidate direction we think the system might evolve along.

---

### Why Try Exponentials $x(t) = v e^{\lambda t}$?

We are dealing with a system that is linear and time-invariant.

That means two things:

* the system’s structure does not change with time
* the rate of change of the state depends linearly on the state itself

In such systems, we are looking for functions whose *rate of change is proportional to their current value*.

An exponential function has exactly this property:

$$
\frac{d}{dt} e^{\lambda t} = \lambda e^{\lambda t}
$$

This means its shape is preserved under differentiation - only its scale changes.

Because the system dynamics relate the derivative of the state directly to the state, any natural mode of behavior must retain its form as time evolves.

This is why exponentials are considered: not as a guess, but because they are the only class of functions that remain structurally consistent under the operation the system performs.

We are therefore searching for directions in state space where the system evolves without changing direction - only growing, decaying, or oscillating in magnitude.

---

### Separating Direction from Time Evolution

To express this idea explicitly, we write the state as:

$$
x(t) = v e^{\lambda t}
$$

Here:

* $v$ represents a fixed direction in the state space
* $e^{\lambda t}$ represents how motion along that direction evolves in time

This form allows us to separate *where the system points* from *how that motion changes with time*.

If such a form satisfies the system equation, then we have identified one of the system’s intrinsic behaviors - a mode of motion that does not depend on external input.

---

Substituting this assumed form into the differential equation:

$$
\dot{x}(t) = A x(t)
$$

gives:

$$
\frac{d}{dt}(v e^{\lambda t}) = A (v e^{\lambda t})
$$

$$
\lambda v e^{\lambda t} = A v e^{\lambda t}
$$

Canceling the exponential term yields:

$$
A v = \lambda v
$$

This is the eigenvalue equation of the state matrix.

---

### What This Equation Is Actually Saying

The equation

$$
A v = \lambda v
$$

states that there exist directions $v$ in the state space that are not rotated into other directions by the system dynamics.

Along such a direction:

* the system does not change shape
* it only scales in magnitude over time

The scalar $\lambda$ determines how that scaling occurs.

These directions are the system’s eigenvectors, and the associated eigenvalues determine the corresponding growth, decay, or oscillation rates.

This is not an abstract algebraic property - it is a direct statement about how the system behaves when its state happens to align with one of these directions.

---

### Geometric Meaning of Eigenvectors

Each eigenvector represents a preferred direction in the system’s state space.

If the system state initially lies along that direction:

* it will remain aligned with that direction for all time
* only its magnitude will change

The full system response can therefore be viewed as a combination of such independent modes.

---

### What the Eigenvalues Tell Us

Let:

$$
\lambda = \sigma + j\omega
$$

Then:

* the real part $\sigma$ determines whether the mode grows or decays
* the imaginary part $\omega$ determines whether the mode oscillates

Specifically:

* $\sigma < 0$: decaying mode → stable
* $\sigma = 0$: marginal mode
* $\sigma > 0$: growing mode → unstable

Each eigenvalue therefore corresponds to a distinct dynamic tendency of the system.

---

### Eigenvalues and Poles

For the system:

$$
\dot{x}(t) = A x(t)
$$

the eigenvalues of $A$ are the system’s poles.

They are defined by the characteristic equation:

$$
\det(sI - A) = 0
$$

which appears identically in the transfer-function formulation.

Thus, poles and eigenvalues are not different objects - they are the same system properties expressed in different mathematical representations.

---

### Right-Half-Plane Eigenvalues and Instability

If any eigenvalue of $A$ has a positive real part, the corresponding mode grows exponentially with time.

This means the system contains at least one internal direction of motion that reinforces itself, even in the absence of external input.

Such a system is open-loop unstable.

---

## Compensating for the Pole Locations of the Plant Itself

### The Pole Locations of the Plant Are Fixed by Its Physics

The poles of the open-loop plant arise from its internal structure: its geometry, parameters, and physical configuration.

In a state-space model,

$$
\dot{x} = A x + B u
$$

these poles are the eigenvalues of the state matrix (A).

If the input is removed,

$$
u(t) = 0
$$

the system reduces to:

$$
\dot{x} = A x
$$

and its behavior is completely determined by those eigenvalues.

No choice of controller alters this internal dynamic directly.
Without feedback, the plant evolves according to its own natural modes.

For example:

* an inverted pendulum possesses an unstable mode by construction
* a system with insufficient damping exhibits oscillatory behavior
* a system with positive feedback embedded in its physics diverges

These behaviors are not design choices - they are properties of the plant itself.

---

### Feedback Does Not Change the Plant - It Modifies the Effective Dynamics

Feedback enters the system through the input channel.

If we apply state feedback,

$$
u = -Kx
$$

the closed-loop dynamics become:

$$
\dot{x} = A x + B u = A x - B K x = (A - B K) x
$$

The plant matrix $A$ has not changed.

What has changed is the *effective* system governing the state evolution.

The matrix $A - BK$ defines a new set of modes that result from the interaction between the plant dynamics and the feedback path.

These are the closed-loop modes.

Feedback therefore does not remove unstable modes from the plant; it reshapes the overall system dynamics so that the resulting modes are stable.

---

### Open-Loop Poles and Closed-Loop Poles Are Different Objects

In transfer-function form, the plant is written as:

$$
G(s) = \frac{N(s)}{D(s)}
$$

The roots of $D(s)$ are the plant poles.

These correspond to the eigenvalues of $A$ in the state-space representation.

When feedback is applied, the system transfer function becomes:

$$
T(s) = \frac{G(s)}{1 + G(s)H(s)}
$$

The poles of the closed-loop system are the roots of:

$$
1 + G(s)H(s) = 0
$$

These are not, in general, the same as the plant poles.

They describe the dynamics of the *entire feedback system*, not the plant in isolation.

---

### What Control Is Actually Doing

The objective of control is not to modify the plant’s physics.

It is to alter the system-level behavior that results from those physics when feedback is present.

If the plant contains unstable modes - represented by right-half-plane poles - then the controller must be designed such that the closed-loop characteristic equation produces stable modes instead.

In state-space terms:

* open-loop behavior is governed by $A$
* closed-loop behavior is governed by $A - BK$

In transfer-function terms:

* plant poles come from $D(s)$
* closed-loop poles come from $1 + G(s)H(s)$

Both descriptions express the same idea:

> stability is determined by the modes of the closed-loop system, not by the plant alone.

---

## The Nyquist Stability Framework

Consider $F(s)$ to be a mapping from points on contour A in the s-plane to complex values in the F-plane:

$$
F(s) = 1 + G(s)H(s)
$$

And define:

$$
T(s) = \frac{G(s)H(s)}{1 + G(s)H(s)} = \frac{\text{Open-loop}}{F(s)}
$$

The roots of $F(s)$ therefore define the characteristic equation of the closed-loop system.

---

### Map Complex Coordinates on Contour A in the RHP of the s-plane to Complex Coordinates in the F-plane that Construct Contour B

Define contour A as a closed loop in the s-plane that encloses the entire RHP, including the imaginary axis and going around any poles on the axis via detours (typical Nyquist construction).

Then evaluate the complex-valued function $F(s)$ at each point on that contour.
As we do this, we map each $s$ in contour A to a corresponding point $F(s)$.

The resulting trace in the F-plane is contour B.

So:
* Contour B is the image of Contour A under the complex function $F(s)$.

---

### We do not want Contour B to Encircle the Origin Because that Would Mean There Exists at Least one Complex Coordinate $s$ in the RHP that Results in $F(s) = 0$

This is the Nyquist argument.

* If contour B encircles the origin, then by the Argument Principle from complex analysis, it implies that:

  * The function $F(s)$ has at least one zero inside the region enclosed by contour A (i.e., the RHP).
  * In other words, some value $s$ in the RHP satisfies $F(s) = 0$.

If contour B encircles the origin, then the closed-loop system has a RHP pole. The encirclement itself does not describe how the pole arises; it indicates only that such a solution exists.

---

> ### Mapping Insight
>
>At each $s$, the value $F(s)$ is a complex number in the F-plane.
Contour B is the tip of that vector as $s$ walks around contour A.\
You are watching the vector: $F(s)$ rotate, stretch, shrink, and possibly loop around the origin.
>
>Contour B is the plot of the values of the characteristic equation $1+G(s)H(s)$ when the complex variable $s$ is restricted to a contour of s-coordinates in the original s-plane - chosen to enclose the right half-plane.

---

### The Zeros of $F(s)$ are the Closed-Loop Poles of $T(s)$

We can write:

$$
T(s) = \frac{G(s)H(s)}{1 + G(s)H(s)} = \frac{G(s)H(s)}{F(s)}
$$

So:

* Wherever $F(s) = 0$, $T(s)$ has a pole.
* These poles are the closed-loop poles of the system.

Hence:

> Zeros of $F(s)$ = closed-loop poles of $T(s)$

This is how Nyquist is used to determine stability without directly computing the closed-loop poles.

---

### Nyquist in G(s)H(s)-Plane

Textbooks formulate the Nyquist criterion slightly differently:

* Instead of mapping $s \mapsto F(s) = 1 + G(s)H(s)$,
* They map $s \mapsto G(s)H(s)$, and check if the resulting contour encircles the point $-1 + j0$.

But it's the same idea - the point $-1$ in the $G(s)H(s)$-plane maps to the origin in the F-plane.

So:

* Encirclement of $\mathbf{-1}$ by the $G(s)H(s)$-plot $\quad\Leftrightarrow\quad$ Encirclement of $\mathbf0$ by the F-plot
* The question is always: *“Does some point in the RHP make* $1 + G(s)H(s) = 0$ *?”*

---
## What Does Nyquist Actually Do?

1. We define a closed contour in the $s$-plane that encloses the entire RHP.
2. We evaluate $G(s)H(s)$ along that contour $\rightarrow$ we get a new curve in the complex plane (called the Nyquist plot).
3. We count how many times that curve encircles the point $-1$.

   * This is the same as counting how many times $1 + G(s)H(s)$ encircles the origin (since zeros of $1 + G(s)H(s)$ correspond to poles of the closed-loop system)
4. Then we use the formula:

$$
N = Z - P
$$

Where:

* $N$ = number of net clockwise encirclements of −1 by the Nyquist plot of $G(s)H(s)$, under the chosen sign convention
* $P$ = number of RHP poles of $G(s)H(s)$ (same as the number of RHP poles in the plant+controller open-loop system)
* We solve for $Z$: the number of RHP closed-loop poles

---

### Insight

We're not using $G(s)H(s)$ to *directly* locate closed-loop poles. We're using it to construct a complex contour that behaves a certain way - and then applying a result from complex analysis (argument principle) that says:

> "The way a function *winds* around a point tells us the number of its zeros inside the domain - even if we don't know where the zeros are."

The encirclement of −1 is not a dynamic mechanism or pole motion. It is geometric evidence that some $s$ in the RHP satisfies:

$$
G(s)H(s) = -1 \quad\Rightarrow\quad 1 + G(s)H(s) = 0 \quad\Rightarrow\quad\text{closed-loop pole in RHP}
$$

---

## Nyquist Conventions

Different control textbooks present the Nyquist criterion using different sign conventions and encirclement definitions.
These differences are not theoretical - they are notational.

All formulations reduce to the same underlying result from complex analysis: the argument principle.

This section fixes a single invariant reference point and then shows how common textbook conventions map onto it.

---

### Universal Anchor (Argument Principle)

The fundamental relation is:

$$
N = Z - P
$$

where:

* $N$ = net counterclockwise encirclements of the critical point
* $Z$ = number of zeros of the characteristic equation in the RHP
  (closed-loop unstable poles)
* $P$ = number of poles of the characteristic equation in the RHP
  (open-loop unstable poles)

This relation is independent of control notation and follows directly from complex analysis.

Throughout this document, counterclockwise encirclement is taken as positive.

Any alternative formulation can be translated back to this form.

---

### Standard Textbook Convention

*(Ogata, Franklin–Powell–Emami-Naeini)*

Most classical control texts present Nyquist using the following interpretation:

* Equation: $N = Z - P$
* Encirclement reference: point $-1 + j0$
* Encirclement sign: counterclockwise counted as positive
* Each open-loop RHP **pole** contributes one **clockwise** rotation of the Nyquist plot
* Each open-loop RHP **zero** contributes one **counterclockwise** rotation

The procedure is:

1. Determine $P$, the number of open-loop RHP poles
2. Count the net counterclockwise encirclements of $-1$
3. Compute the number of closed-loop RHP poles using:

$$
Z = N + P
$$

This formulation is fully consistent with the argument principle when the sign convention is interpreted correctly.

---

### Nise’s Textbook Convention

Nise presents the same underlying relationship using a different algebraic labeling.

* Encirclement sign: counterclockwise remains positive
* The interpretation of pole and zero contributions is reversed
* The algebraic form corresponds to $P - Z$ rather than $Z - P$

The theory is unchanged; only the labeling differs.

To translate Nise’s formulation into the universal form:

1. Reverse the sign of any encirclement or phase contribution
   since $(P - Z = -(Z - P))$
2. Apply the standard argument-principle relation:

$$
N = Z - P
$$

Once translated, the stability conclusions are identical.

---

## Inverted Pendulum Example

Consider an unstable plant modeled as:

$$
G(s) = \frac{1}{s - a}, \quad a > 0
$$

This plant contains a right-half-plane pole at $s = a$, corresponding to an unstable open-loop mode.

Before any feedback is applied, the system therefore has:

$$
P = 1
$$

open-loop RHP pole.

Now suppose a controller $H(s) = C(s)$ is introduced with the intent of stabilizing the system.

The resulting loop transfer function is:

$$
G(s)H(s) = \frac{C(s)}{s - a}
$$

At this point, nothing about the plant’s internal dynamics has changed.
The unstable mode still exists.

The role of feedback is not to remove this mode, but to determine whether the closed-loop characteristic equation results in unstable solutions.

---

### Applying the Nyquist Criterion

To evaluate stability, we proceed as follows:

1. Count the number of open-loop RHP poles of $G(s)H(s)$

   
$$
P = 1
$$

2. Construct the Nyquist plot of $G(s)H(s)$

3. Count the net encirclements of the critical point $-1$

Using the Nyquist relation:

$$
N = Z - P
$$

we infer the number of closed-loop RHP poles $Z$.

---

### Interpretation of the Result

* If the Nyquist plot produces **one clockwise encirclement** of $-1$:

$$
N = -1 \quad\Rightarrow\quad Z = 0
$$

The closed-loop system has no unstable modes and is therefore stable.

* If the Nyquist plot produces **no encirclement**:

$$
N = 0 \quad\Rightarrow\quad Z = 1
$$

The closed-loop system retains one unstable mode and remains unstable.

In both cases, Nyquist does not locate poles or describe how instability develops.

It answers a single question:

> Does the closed-loop characteristic equation
> $$
> 1 + G(s)H(s) = 0
> $$
> admit solutions in the right half-plane?

---

### What the Example Demonstrates

This example illustrates how Nyquist is used in practice:

* the number of unstable modes present before feedback is known in advance
* the Nyquist plot provides geometric evidence about the existence of unstable closed-loop modes
* stability is determined entirely by the inferred value of $Z$

If $Z = 0$, feedback has successfully suppressed the unstable behavior.

If $Z > 0$, the unstable mode persists.

Nyquist does not describe transient behavior, pole migration, or time-domain response.

It infers the presence or absence of unstable modes in the closed-loop system.

---

At this point, the remaining question is not mathematical, but physical: why does phase delay determine whether feedback suppresses or reinforces motion?

---

## Phase, Accumulation, and Turning Points

Phase behavior in feedback systems arises from physical accumulation.

Many system variables do not respond directly to input, but to the time integral of upstream quantities. Examples include position as the integral of velocity, capacitor voltage as the integral of current, and inductor current as the integral of voltage.

These relationships impose a fixed causal ordering:

$$
\text{input}\quad\rightarrow\quad\text{rate}\quad\rightarrow\quad\text{accumulated state}
$$

Because accumulated quantities represent stored history, they cannot change direction until the quantities driving them have already done so.

As a result, turning points in accumulated states occur later in time than turning points in their corresponding rates. This is a geometric property of smooth functions and does not depend on modeling framework.

This delay is the physical origin of phase lag.

---

### Poles as Accumulation

In linear system models, accumulation appears as integration.

In the Laplace domain, integration corresponds to division by $s$, which introduces a pole in the transfer function. A pole therefore represents a physical storage process within the system.

Each pole places the system’s response further downstream in the accumulation chain.

Because accumulated variables reach their turning points later, poles introduce phase lag. This lag reflects the delay between the onset of a disturbance and the system’s ability to respond to it.

---

### Zeros as Rate Sensitivity

Zeros represent the complementary effect.

In the Laplace domain, multiplication by $s$ corresponds to differentiation in time. Differentiation emphasizes rate of change rather than accumulated state.

A zero therefore increases the system’s sensitivity to upstream quantities in the causal chain - such as velocity, current, or slope - which evolve earlier in time than the states they ultimately produce.

By responding to rate information, the system reacts earlier within a motion cycle. This produces phase lead.

This behavior does not imply prediction or foresight. The response occurs earlier because the measured quantity itself exists earlier in the system’s causal progression.

---

### Turning Points and Feedback Timing

Feedback stability depends on whether corrective action arrives before or after the system passes a turning point.

If feedback arrives after the accumulated state has already reversed direction, the correction reinforces motion rather than opposing it. If feedback acts earlier - while the rate is still changing - it suppresses motion.

Phase margin therefore reflects how much accumulation delay exists between sensing and actuation.

Viewed physically, phase margin is a measure of how far downstream in the accumulation chain the feedback signal is applied.

---

### Summary

* Accumulation delays turning points
* Poles represent accumulation and introduce phase lag
* Zeros increase rate sensitivity and introduce phase lead
* Stability depends on whether feedback acts before or after critical turning points

This interpretation remains consistent across time-domain, frequency-domain, and state-space descriptions.

---

## Phase, Accumulation, and the Meaning of Nyquist

The Nyquist criterion does not introduce a new physical mechanism. It evaluates the consequences of phase accumulation already present in the system.

Phase lag arises because physical systems accumulate energy and state over time. Each layer of accumulation - represented mathematically by integration - delays the system’s turning points. Poles therefore correspond to physical storage processes, and each contributes additional delay between disturbance and corrective response.

Zeros introduce the complementary effect by making the system sensitive to rates of change. Rate information appears earlier in time than accumulated state, so zeros reduce effective delay and introduce phase lead.

From this perspective, phase is not an abstract frequency-domain quantity. It represents the timing relationship between when a disturbance begins to grow and when feedback-generated correction returns to oppose it.

Nyquist analysis examines whether, for any system mode that satisfies the characteristic equation, the accumulated phase delay through the loop is sufficient to invert the intended sign of feedback. When the loop transmission reaches a phase shift of approximately $180^\circ$ with magnitude equal to or exceeding unity, corrective action arrives aligned with - rather than opposing - the system’s motion.

The Nyquist plot does not describe how instability develops in time. It determines whether the loop contains conditions under which accumulated delay converts negative feedback into reinforcement.

Encirclement of the critical point therefore indicates not pole motion, but the existence of a closed-loop mode for which phase accumulation causes feedback to act too late to stabilize the system.

---

## What Does Feedback *Do*, Physically?

### In an Open-Loop Unstable System (Like an Inverted Pendulum):

* A disturbance causes a small deviation
* That deviation grows exponentially, because the system dynamics reinforce it

If we do nothing, the pendulum falls - amplitude grows in the unstable mode.

### When We Apply Negative Feedback:

* We observe the output, process it through $H(s)$, and subtract it from the input (negative feedback)
* This creates a corrective response that attempts to oppose deviations
* But if that correction is too slow, delayed, attenuated, or phase-lagged, it arrives late or in the wrong direction, and the deviation is reinforced rather than suppressed

This is the origin of instability in a feedback system: when the loop is configured to stabilize, but delay or phase shift turns correction into positive reinforcement.

So the core feedback dynamic is:

> “How much of the signal that I fed forward comes back to oppose it - and with what delay, direction, and magnitude?”

This is where time-domain intuition becomes less transparent, and frequency-domain representations become more useful.

---

### One Traversal Around the Loop = One Round-Trip Energy Return

>*This language is interpretive rather than literal; it is used to describe observable system behavior rather than conserved physical energy.

Now imagine a disturbance enters the loop at some frequency $\omega$. It goes through:

* The plant $G(j\omega)$: dynamics of our physical system
* The feedback $H(j\omega)$: sensor and controller response
* The signal returns to the summing junction to oppose the disturbance

If the magnitude of the return is > 1 and the phase shift is $180^\circ$, then instead of opposing the disturbance, the return adds to it - perfect positive feedback. That’s the loop transmission being equal to −1.

That is the physical meaning of:

$$
G(j\omega)H(j\omega) = -1
\quad\Rightarrow\quad
1 + G(j\omega)H(j\omega) = 0
$$

> The loop has exactly enough gain and phase shift to turn a small disturbance into a self-reinforcing oscillation.

So encircling −1 in the Nyquist plane means that there exists a value of $ s $ in the RHP for which the feedback configuration reinforces the system’s response rather than suppressing it.

---

### The Argument Principle = Stability as Energy Phase Flow

The argument principle tracks:

> How many times the system response vector (a complex number) rotates around a critical failure point (−1).

Interpreted physically, this corresponds to:

* Sweeping through frequencies (or, more generally, the RHP contour in Laplace space)
* Tracking how the loop responds at each frequency
* Watching phase accumulation and delay at each frequency
* Checking whether the net response at any frequency aligns to cause perfect reinforcement (i.e., maps to −1)

The number of full rotations around −1 reflects:

> Whether the loop contains conditions under which small perturbations become self-reinforcing.

If we know how many unstable poles we started with (open-loop), and we can observe how the loop’s return signal winds around the danger point (−1), then we can determine whether those unstable tendencies are suppressed or amplified by feedback.

Nyquist is the expression of the physical truth:

> Feedback either suppresses or reinforces motion - depending on delay, phase, and gain alignment. Nyquist indicates whether the conditions for reinforcement exist, even when we can’t solve the system analytically.

---

### Back to the Inverted Pendulum

Consider this:

* We apply small oscillatory disturbances to an inverted pendulum control system
* We vary the frequency of the disturbance
* We observe the phase lag and gain of the loop’s response at each frequency

As we sweep through frequencies:

* There may be ranges where the loop delay and dynamics align such that:

  * the controller acts too slowly
  * or with excessive phase lag
  * so that corrective action becomes reinforcing rather than suppressive

This corresponds to alignment near the critical −1 condition.

That is what encircling −1 indicates:

> That the closed-loop system results in one or more modes whose feedback gain and phase configuration lead to self-reinforcing response growth.

---

### When the Plant Has Multiple RHP Poles:

* The system contains multiple unstable modes
* Each mode exhibits exponential growth when excited
* The role of feedback is to suppress these growing modes

We cannot simply “damp out energy” in a generic sense - stability requires that each unstable mode be addressed through the feedback dynamics.

Each RHP pole corresponds to an independent direction of instability in the system’s state space.
For the closed-loop system to be stable, all such directions must be stabilized by the feedback configuration.

---
## A Physics-Based Naming Convention for the Argument Principle

*This notation is a conceptual relabeling of the standard Nyquist variables, not a modification of the argument principle.*

### Variables and Physical Meaning

Let’s use physically meaningful notation:

| Symbol            | Meaning                                                                 |
| ----------------- | ----------------------------------------------------------------------- |
|$P_{\text{orig}}$ | Number of open-loop RHP poles (unstable modes before feedback)     |
|$P_{\text{fb}}$   | Number of closed-loop RHP poles (unstable modes after feedback)    |
|$N$               | Net effect of feedback on system stability                            |

Then the physically grounded version of the Nyquist equation is:

$$
\boxed{P_{\text{fb}} = N + P_{\text{orig}}}
$$

---

### Physical Meaning of Each Term

#### $P_{\text{orig}}$: Instability Before Feedback

* These correspond to unstable modes whose amplitudes grow exponentially
* Examples include inverted pendulums, runaway thermal systems, or amplifiers with excessive gain and delay
* Without feedback, these modes lead to divergent system behavior

---

### Feedback Loop (Origin of $N$)

* The controller senses the output and feeds a processed version back into the input
* The feedback’s gain and timing (phase) determine whether it suppresses or reinforces unstable modes

The feedback loop is analogous to pushing a swing:

* Push in phase → motion grows (reinforcement)
* Push opposite phase → motion decays (suppression)

So:

* $N < 0$ → feedback suppresses unstable modes (stabilizing)
* $N > 0$ → feedback reinforces unstable modes (destabilizing)
* $N = 0$ → feedback does not change stability

---

### $P_{\text{fb}}$: Instability After Feedback

* This is the number of unstable modes remaining in the closed-loop system
* For stability, we require:

$$
\boxed{P_{\text{fb}} = 0}
$$

---

### Stability Condition

$$
\boxed{P_{\text{fb}} = N + P_{\text{orig}}}
$$

To achieve stability:

$$
\boxed{N = -P_{\text{orig}}}
$$

That is, feedback must suppress exactly as many unstable modes as are present in the plant.

---

### Intuition Behind a Physics-Based Naming Convention

* $N$ is not a geometric rotation to memorize - it represents the net stabilizing or destabilizing effect of feedback
* In standard notation, $Z$ denotes closed-loop unstable poles; here this meaning is made explicit
* Open-loop poles are not canceled by zeros - feedback reshapes the system dynamics
* $P_{\text{orig}}$ counts unstable modes before feedback
* $N$ reflects whether feedback improves, degrades, or leaves stability unchanged

> The essential question is not geometric:
>
> **Did the feedback suppress unstable modes - or reinforce them?**

---

## A Shaded Bar Model of Feedback Modes

*This shaded-bar representation originated as a personal mental visualization developed to reason about phase and gain margin behavior, and is retained here because it continues to provide useful intuition when interpreted alongside the formal mathematics.*

Feedback behavior can be interpreted as a blend of negative and positive effects, where some portion suppresses the input and some portion reinforces it, depending on the phase angle.

---
### The Core of the Analogy

Imagine feedback effectiveness as a horizontal bar from 0° to 180° of phase lag:

* At $0^\circ$: Entire bar is shaded → 100% negative feedback
* At $90^\circ$: Half shaded → 50% negative, 50% positive
* At $180^\circ$: All unshaded → 100% positive feedback
* Everything in between is a blend:
  More lag → Less suppression (less shaded) → More reinforcement (more unshaded)

The *net feedback effect* transitions continuously as phase lag increases.

Let’s back this up from first principles using vector projection.

---

### Feedback = Vector Projection

We revisit the subtraction:

$$
e(t) = \text{input}(t) - M \cdot \text{feedback}(t)
$$

Let’s model the feedback phasor with magnitude 1 and phase lag of $ \phi $, so the projection of that vector onto the input axis (0°) is:

$$
\text{Effective Suppression} = -\cos(\phi)
$$

This cosine curve gives the net suppression effect:

| Phase $ \phi $ | $ -\cos(\phi) $ | Interpretation |
| ---------------- | ------------------ | -------------- |
| $0^\circ$      | −1                 | Fully subtractive (100% shaded) |
| $30^\circ$     | ≈ −0.87            | Mostly subtractive |
| $90^\circ$     | 0                  | Neutral |
| $150^\circ$    | ≈ +0.87            | Mostly additive |
| $180^\circ$    | +1                 | Fully additive (0% shaded) |

This directly matches the shaded-bar analogy:

* The shaded portion represents the subtractive component of feedback
* The unshaded portion represents the reinforcing component

The bar analogy is a qualitative visualization of the cosine projection, providing intuition for how feedback effectiveness varies with phase.

---

### Why the System Still Isn’t Unstable at $90^\circ$

Even though a reinforcing component appears past $90^\circ$, the system does not exhibit exponential growth unless:

* the reinforcing component aligns (phase approaches $180^\circ$)
* and the loop gain magnitude is sufficiently large (≥ 1)

So:

* feedback transitions continuously with phase
* the balance between suppression and reinforcement shifts smoothly
* instability occurs only when reinforcing alignment and gain dominate

---

### Recap

* The effect of feedback on system stability varies smoothly with phase.
* It can be interpreted as a “feedback blend,” where:
  * From $0^\circ$ to $90^\circ$, suppressive feedback dominates
  * At $90^\circ$, the net feedback effect is neutral
  * From $90^\circ$ to $180^\circ$, reinforcing influence dominates
  * At $180^\circ$, feedback is fully reinforcing - any loop gain ≥ 1 leads to instability

The shaded-bar model is a visual tool: it compresses a trigonometric relationship into a straightforward physical intuition.

---

## Closing

This document defines a working model for reasoning about feedback stability across multiple analytical representations.

The model is constrained by established mathematics and control theory and is intended to remain consistent across state-space, transfer-function, and frequency-domain views.

It serves as a reference framework for continued analysis and implementation.

---
