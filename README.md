Download Link: https://assignmentchef.com/product/solved-csce790-002-deep-reinforcement-learning-and-search-homework-1
<br>
<h1>Installation</h1>

We will be using the conda package management system.

To get started, download Anaconda from https://docs.anaconda.com/anaconda/install/.

After downloading the code for the homework, cd to the directory of the code.

A list of all the packages needed for the virtual environment is in spec-file.txt. Duplicate the virtual environment with: conda create –name &lt;your_environment_name&gt; –file spec-file.txt

Then, activate your environment conda activate &lt;your_environment_name&gt;

When you are finished using the environment you can deactivate it with: conda deactivate

You can always activate it again with conda activate &lt;your_environment_name&gt;

<h1>Running the Code</h1>

You can run the code with the command python run_assignment_1.py –map maps/map1.txt. You should see a visualization of the AI Farm environment. The code will output the ∆ from Algorithm 2 at every step and output DONE when value iteration has converged.

There are switches that you can use

<ul>

 <li>–discount, to change the discount (default=1.0)</li>

 <li>–no_text, to omit text shown on the figure</li>

 <li>–no_val, to omit the state-value shown on the figure</li>

 <li>–no_policy, to omit the policy shown on the figure</li>

 <li>–rand_right, to change the probability that the wind blows you to the right (default=0.0)</li>

 <li>–wait, the number of seconds to wait after every iteration so that you can visualize your algorithm (default=0.0)</li>

</ul>

i.e. python run_assignment_1.py –map maps/map1.txt –no_text –rand_right 0.1 –wait 0.5

<h1>Helper Functions</h1>

In your implementations, you are given an Environment object env. You will need the env.get_actions() function that returns a list of all possible actions. You will also need env.state_action_dynamics(state, action), which returns, in this order, the expected return <em>r</em>(<em>s,a</em>), all possible next states given the current state and action, their probabilities.

Keep in mind, while the notation for value iteration update, <em>V </em>(<em>s</em>) ← max<em><sub>a</sub></em>(<em>r</em>(<em>s,a</em>) + <em>γ </em><sup>P</sup><em><sub>s</sub></em>0 <em>p</em>(<em>s</em><sup>0</sup>|<em>s,a</em>)<em>V </em>(<em>s</em><sup>0</sup>)), sums over all possible states, env.state_action_dynamics(state, action) omits all states that have a state-transition probability of zero. This significantly reduces the number of elements in the summation.

<h1>Part 1: Value Iteration and the Optimal Policy</h1>

<h2>Part 1.1: Value Iteration (35 pts)</h2>

Value iteration can be used to find, or approximate, the optimal value function. In this exercise, we will be finding the optimal value function, exactly.

Implement value_iteration_step in assignments_code/assignment1.py.

Upon running python run_assignment_1.py –map maps/map1.txt, Algorithm 1 will automatically begin running with the value <em>θ </em>set to 0. It will automatically call your code, Algorithm 2.

Vary the environment dynamics by making the environment stochastic –rand_right to 0.1 and 0.5.

To see a step-by-step example of what a correct implementation of value iteration should look like, as well as the resulting state-value function and policy for the stochastic case, please see the slides on Markov Decision Processes.

<strong>Algorithm 1 </strong>Value Iteration

1: <strong>procedure </strong>Value Iteration(S<em>,V,θ,γ</em>)

2:              ∆ ← inf

3:               <strong>while </strong>∆ <em>&gt; θ </em><strong>do</strong>

4:                       ∆<em>,V </em>= Value Iteration Step(S<em>,V </em>)

5:             <strong>end while</strong>

6:              <strong>return </strong><em>V                                                                                                                 . </em>Approximation of <em>v</em><sub>∗</sub>

7: <strong>end procedure</strong>

<h2>Part 1.2: Obtaining the Optimal Policy (35 pts)</h2>

After finding the optimal value function <em>v</em><sub>∗</sub>, we can then use it to find the optimal policy using 1. Implement get_action in assignments_code/assignment1.py so that it implements 1.

<em>π</em><sup>∗</sup>(<em>s</em>) = argmax(<em>r</em>(<em>s,a</em>) + <em>γ </em><sup>X</sup><em>p</em>(<em>s</em><sup>0</sup>|<em>s,a</em>)<em>v</em><sub>∗</sub>(<em>s</em><sup>0</sup>))                                               (1)

<em>a</em>

<em>s</em>0

<strong>Algorithm 2 </strong>Value Iteration Step

1: <strong>procedure </strong>Value Iteration Step(S<em>,V,aγ</em>)

2:              ∆ ← 0

3:               <strong>for </strong><em>s </em>∈ S <strong>do</strong>

4:                      <em>v </em>← <em>V </em>(<em>s</em>)

5:                          <em>V </em>(<em>s</em>) ← max<em><sub>a</sub></em>(<em>r</em>(<em>s,a</em>) + <em>γ </em><sup>P</sup><em><sub>s</sub></em>0 <em>p</em>(<em>s</em><sup>0</sup>|<em>s,a</em>)<em>V </em>(<em>s</em><sup>0</sup>))

6:                        ∆ ← max(∆<em>,</em>|<em>v </em>− <em>V </em>(<em>s</em>)|)

7:             <strong>end for</strong>

8:               <strong>return </strong>∆<em>,V</em>

9: <strong>end procedure</strong>

<strong>Part 1: What to Turn In</strong>

Turn in your implementation of assignments_code/assignment1.py.

<h1>Part 2: Comparison to Sutton and Barto’s Notation (30 pts)</h1>

You will notice that the state-value assignment step in Algorithm 2 is written differently than in the value iteration algorithm on page 83 of <em>Reinforcement Learning: An Introduction </em>(http://incompleteideas.net/book/RLbook2020.pdf):

<table width="505">

 <tbody>

  <tr>

   <td width="485"><em>V </em>(<em>s</em>) ← max(<em>r</em>(<em>s,a</em>) + <em>γ </em><sup>X</sup><em>p</em>(<em>s</em><sup>0</sup>|<em>s,a</em>)<em>V </em>(<em>s</em><sup>0</sup>))<em>a</em><em>s</em>0</td>

   <td width="20">(2)</td>

  </tr>

  <tr>

   <td width="485"><em>V </em>(<em>s</em>) ← max<sup>X</sup><em>p</em>(<em>s</em><sup>0</sup><em>,r</em>|<em>s,a</em>)(<em>r </em>+ <em>γV </em>(<em>s</em><sup>0</sup>))</td>

   <td width="20">(3)</td>

  </tr>

 </tbody>

</table>

<em>a</em>

<em>s</em><sup>0</sup><em>,r</em>

Show that these two are equivalent by rearranging 3 so that it looks like 2. Use the definitions provided in the lecture slides on Markov Decision Processes (also shown in Equations 3.4 and 3.5 in <em>Reinforcement Learning: An Introduction</em>). Show your work.

<strong>Part 2: What to Turn In</strong>

Turn in a PDF of your work.