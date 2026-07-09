---
marp: true
paginate: true
---

<!-- These set styles for the entire document. -->
<style>

h1 {
  /* background: linear-gradient(90.2deg, rgba(190, 70, 102, 0.93) -15.6%, rgb(252, 154, 154) -15.6%, rgba(190, 70, 102, 0.92) 17.9%, rgb(58, 13, 48) 81.6%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent; */
  color: rgba(190, 70, 102, 0.92);
}

h1 { font-size: 72px }

h2 { font-size: 48px; color: #fff; }

h3 { font-size: 36px; vertical-align: top; }

h3, h4, h5 {
  /* background: rgb(58, 13, 48);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent; */
  color: rgb(58, 13, 48);
}

h3, h4, h5 { color: #444 }

section {
  position: absolute;
  height: inherit;
  vertical-align: bottom;
  margin: 0;
  display: table-cell;
}

/* section h3 {
    position: absolute;
    top: 50px;
  } */

.imgContainer{
    float:left;
}

.txtContainer{
    float:right;
}

</style>

<br><br><br>

##### Lecture 3

# More Simple Linear Regression

#### DSC 40A, Summer 2024

---

### Announcements

- Homework 1 is due **tonight**, 11:59PM PT. 
- Homework 2 is due tomorrow, **Tuesday**, at 11:59PM PT.

  - We are aiming to give you feedback on HW 1 before HW 2 is due on Tuesday, keep an eye out tomorrow.

- Week 2 picks up the pace really quickly, please prepare to have more of a workload this week as we start prepare for your upcoming exam.

- Groupwork 1 solutions are available on [Piazza](https://piazza.com/class/mqzuizbulee109/post/29).

---

### Exam Announcements

- Midterm Exam Logistics will be released this week, please keep an eye on your inbox for those. We will be doing live proctoring and you will be expected to be on Zoom with camera and sound on. More details to come ASAP.
  - Many of you are in non-PT time zones. Please plan to take the exam during the expected exam time slot: Wednesday, July 15th, 11:00AM-1:50PM, PT. 
  - We will **not** be accommodating different exam times, unless you have OSD accommodations specifically for that.

- If you intend to use your exam accommodations on this upcoming midterm, please make sure OSD sends your AFA letter to us **by this Friday, July 10th**. 
  - It's hard to change/reschedule your exam last minute, so giving us at least the weekend to rearrange and make sure we can accommodate you is really helpful.

---

### Agenda - Part 1

- Recap: Simple linear regression.
- Correlation.
- Interpreting the formulas.
- Connections to related models.
- Introduction to linear algebra.

---

<br><br><br>

##### Lecture 3 Part 1

# More Simple Linear Regression

#### DSC 40A, Summer 2026

---
<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Ask in Q&A, respond to questions in chat.**

<br><br><br>

<center><big><b>Remember, you can always ask questions!</b></big>

</center>

---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Recap: Simple linear regression

---

### Recap

<div style="width: 100%;">
    <div style="width: 50%"> 
    <br>
<img src="imgs/scatter-1.png" width=50% align="left">
    </div>
    <div style="margin-left: 50%; height: 100px;" markdown="1"> 


- In Lecture 2 part 1, our goal was to fit a **simple linear regression** model, $H(x) = w_0 + w_1 x$, to our commute times dataset.
    - $x_i$: The $i$th home departure time (e.g. 8.5, for 8:30 AM).
    - $y_i$: The $i$th actual commute time (e.g. 76 minutes).
    - $H(x_i)$: The $i$th predicted commute time.
- To do so, we used squared loss.

</div>
</div>

---

### The modeling recipe

1. Choose a model.

<br><br>

2. Choose a loss function.

<br><br>

3. Minimize average loss to find optimal model parameters.

<br>
<br>
<br>

---

### Least squares solutions

- Our goal was to find the parameters $w_0^*$ and $w_1$* that minimized:

$$R_\text{sq}(w_0, w_1) = \frac{1}{n} \sum_{i = 1}^n \left( y_i - (w_0 + w_1 x_i) \right)^2$$

- To do so, we used calculus, and we found that the minimizing values are:

$$
\boxed{\begin{align*}
w_1^* &=
    \frac{
        \displaystyle
        \sum_{i=1}^n (x_i - \bar x)(y_i - \bar y)
    }{
        \displaystyle
        \sum_{i=1}^n (x_i - \bar x)^2
    }
    \qquad
    \qquad
w_0^* =
    \bar y - w_1^* \bar x
\end{align*}}
$$

- We say $w_0^*$ and $w_1^*$ are **optimal parameters**, and the resulting line is called the **regression line**.

---


<center>

<img src="imgs/model.png" width=65%>

</center>


---

### Now what?

We've found the optimal slope and intercept for linear hypothesis functions using squared loss (i.e. for the regression line). Now, we'll:

- See how the formulas we just derived connect to the formulas for the slope and intercept of the regression line we saw in DSC 10.
  - They're the same, but we need to do a bit of work to prove that.
- Learn how to interpret the slope of the regression line.
- Understand connections to other related models.
- Learn how to build regression models with **multiple inputs**.
  - To do this, we'll need linear algebra!


---

<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat**

Consider a dataset with just two points, $(2, 5)$ and $(4, 15)$. Suppose we want to fit a linear hypothesis function to this dataset using squared loss. What are the values of $w_0^*$ and $w_1^*$ that minimize empirical risk?

- A. $w_0^* = 2$, $w_1^* = 5$
- B. $w_0^* = 3$, $w_1^* = 10$
- C. $w_0^* = -2$, $w_1^* = 5$
- D. $w_0^* = -5$, $w_1^* = 5$


---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Correlation

---

### Quantifying patterns in scatter plots

<div style="width: 100%;">
   <div style="width: 50%">
   <br>
<img src="imgs/correlations1.png" width=50% align="right">
   </div>
   <div style="margin-left: 50%; height: 100px;" markdown="1">


- In DSC 10, you were introduced to the idea of the **correlation coefficient**, $r$.
- It is a measure of the strength of the **linear association** of two variables, $x$ and $y$.
- Intuitively, it measures how tightly clustered a scatter plot is around a straight line.
- It ranges between -1 and 1. 


</div>
</div>


---

### The correlation coefficient

- The correlation coefficient, $r$, is defined as the **average of the product of $x$ and $y$, when both are in standard units**.
- Let $\sigma_x$ be the standard deviation of the $x_i$s, and $\bar{x}$ be the mean of the $x_i$s.
- $x_i$ in standard units is $\frac{x_i - \bar{x}}{\sigma_x}$.
- The correlation coefficient, then, is:

$$r = \frac{1}{n} \sum_{i = 1}^n \left( \frac{x_i - \bar{x}}{\sigma_x} \right) \left( \frac{y_i - \bar{y}}{\sigma_y} \right) $$



---

### The correlation coefficient, visualized

<br>


<center>

<img src="imgs/correlations.png" width=80%>

</center>

---

### Another way to express $w_1^*$

- It turns out that $w_1^*$, the optimal slope for the linear hypothesis function when using squared loss (i.e. the regression line), can be written in terms of $r$!

    $$w_1^* = \frac{
                        \displaystyle
                        \sum_{i=1}^n (x_i - \bar x)(y_i - \bar y)
                    }{
                        \displaystyle
                        \sum_{i=1}^n (x_i - \bar x)^2
                    } = r \frac{\sigma_y}{\sigma_x}$$

- It's not surprising that $r$ is related to $w_1^*$, since $r$ is a measure of linear association.

- Concise way of writing $w_0^*$ and $w_1^*$:

    $$w_1^* = r\frac{\sigma_y}{\sigma_x} \qquad w_0^* = \bar{y} - w_1^* \bar{x}$$


---

### Proof that $w_1^* = r\frac{\sigma_y}{\sigma_x}$

<br><br><br><br><br><br><br><br><br><br><br><br>

---

Let's test these new formulas out in code! Follow along [here](https://drive.google.com/file/d/1m1qsNYdDofKlPqH7MiagIQDRCLpDl-6f/view?usp=sharing).

<br>

<center>

<img src="imgs/model.png" width=60%>

</center>

---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Interpreting the formulas

---

### Interpreting the slope

$$w_1^* = r \frac{\sigma_y}{\sigma_x}$$

- The units of the slope are **units of $y$ per units of $x$**.
- In our commute times example, in $H(x) = 142.25 - 8.19x$, our predicted commute time **decreases by 8.19 minutes per hour**.

<br><br><br><br><br><br><br>


---

### Interpreting the slope

$$w_1^* = r \frac{\sigma_y}{\sigma_x}$$

<center>

<img src="imgs/default.png" width=350><img src="imgs/spready.png" width=350><img src="imgs/spreadx.png" width=350>

</center>

- Since $\sigma_x \geq 0$ and $\sigma_y \geq 0$, the slope's sign is $r$'s sign.

- As the $y$ values get more spread out, $\sigma_y$ increases, so the slope gets steeper.

- As the $x$ values get more spread out, $\sigma_x$ increases, so the slope gets shallower.

---

### Interpreting the intercept

<br>
<img src="imgs/model.png" style="margin-right: 100px;" align="left" width=50%>
<div align="right" style="height: 100px;" markdown="1"> 

$w_0^* = \bar{y} - w_1^* \bar{x}$

<br>

- What are the units of the intercept?<br><br><br>
- What is the value of $H^*(\bar{x})$?

</div>

---

<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat.**

We fit a regression line to predict commute times given departure hour. Then, we add 75 minutes to all commute times in our dataset. What happens to the resulting regression line?

- A. Slope increases, intercept increases.
- B. Slope decreases, intercept increases.
- C. Slope stays the same, intercept increases.
- D. Slope stays the same, intercept stays the same.

---

### Correlation and mean squared error (Proof in FAQs page)

- **Claim**: Suppose that $w_0^*$ and $w_1^*$ are the optimal intercept and slope for the regression line. Then,

$${\color{purple}R_\text{sq}(w_0^*, w_1^*)} = \sigma_y^2 (1 - {\color{blue}r}^2)$$

- That is, the <span style="color:purple">mean squared error of the regression line's predictions</span> and the correlation coefficient, ${\color{blue}r}$, always satisfy the relationship above.

- Even if it's true, why do we care?
    - In machine learning, we often use both the <span style="color:purple">mean squared error</span> and ${\color{blue}r}^2$ to compare the performances of different models.
    - If we can prove the above statement, we can show that **finding models that minimize <span style="color:purple">mean squared error</span>** is equivalent to **finding models that maximize ${\color{blue}r}^2$**.

<!-- ---

### Proof that $R_\text{sq}(w_0^*, w_1^*) = \sigma_y^2 (1 - r^2)$

<br><br><br><br><br><br><br><br><br><br><br><br>

---
 -->

---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Connections to related models

---


<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat.**

Suppose we chose the model $H(x) = w_1 x$ and squared loss.<br>What is the optimal model parameter, $w_1^*$?


<div style="width: 100%;">
<div class=imgContainer style="width: 50%; float: left">

- A. $\displaystyle \frac{\sum_{i = 1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i = 1}^n (x_i - \bar{x})^2}$

- B. $\displaystyle \frac{\sum_{i = 1}^n x_iy_i}{\sum_{i = 1}^n x_i^2}$

</div>

<div class=txtContainer style="height: 100px;" markdown="1"> 

- C. $\displaystyle \frac{\sum_{i = 1}^n x_iy_i}{\sum_{i = 1}^n x_i^2}$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

- D. $\displaystyle \frac{\sum_{i = 1}^n y_i}{\sum_{i = 1}^n x_i}$

</div>
</div>


---

### Exercise

Suppose we chose the model $H(x) = w_1 x$ and squared loss.<br>What is the optimal model parameter, $w_1^*$?

<br><br>
<br><br>
<br><br>
<br><br>


---

<center>

<img src="imgs/both-models.png" width=75%>

</center>

---

### Exercise

Suppose we choose the model $H(x) = w_0$ and squared loss.<br>What is the optimal model parameter, $w_0^*$?

<br><br>
<br><br>
<br><br>
<br><br>


---

### Comparing mean squared errors

- With both:
    - the constant model, $H(x) = h$, and 
    - the simple linear regression model, $H(x) = w_0 + w_1x$, 
   
   when we chose squared loss, we minimized mean squared error to find optimal parameters:

$$R_\text{sq}(H) = \frac{1}{n} \sum_{i = 1}^n \left( y_i - H(x_i) \right)^2$$

- **Which model minimizes mean squared error more?**

---

### Comparing mean squared errors

<div style="width: 100%;">
<div style="width: 65%; float: left"> 
<center>

<img src="imgs/constant-to-slr.png" class=imgContainer width=60%>

</center>

</div>
    <div style="margin-left: 66%; height: 100px;" markdown="1"> 


$\text{MSE} = \frac{1}{n} \sum_{i = 1}^n \left( y_i - H(x_i) \right)^2$

- The MSE of the best <span style="color:red">simple linear regression model</span> is $\approx 97$.
- The MSE of the best <span style="color:purple">constant model</span> is $\approx 167$.
- The <span style="color:red">simple linear regression model</span> is a more flexible version of the <span style="color:purple">constant model</span>.

</div>
</div>



---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Linear algebra review

---

### Wait... why do we need linear algebra?

- Soon, we'll want to make predictions using more than one feature.
    - Example: Predicting commute times using departure hour and temperature.
- Thinking about linear regression in terms of **matrices and vectors** will allow us to find hypothesis functions that:
    - Use multiple features (input variables).
    - Are non-linear, e.g. $H(x) = w_0 + w_1 x + w_2 x^2$.
- Before we dive in, let's review.

<br><br>
<br><br>


---

### Spans of vectors


- One of the most important ideas you'll need to remember from linear algebra is the concept of the **span** of two or more vectors.
- To jump start our review of linear algebra, let's start by watching [🎥 **this video by <span style="color:blue">3blue</span><span style="color:brown">1brown</span>**](https://youtu.be/k7RM-ot2NWY?si=HRt1nlqIWJPLI0cp).

<center>

<img src="imgs/span.png" width=50%>

</center>

---

### Next time

- We'll review the necessary linear algebra prerequisites.
- We'll then start to formulate the problem of minimizing mean squared error for the simple linear regression model **using matrices and vectors**.

<br><br>
<br><br>
<br><br>


---

# Break

---

<br><br><br>

##### Lecture 3 Part 2

# Dot Products and Projections

#### DSC 40A, Summer 2026

---


### Agenda for Part 2

- Recap: Friends of simple linear regression.
- Dot products.
- Spans and projections.


---

<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat**

<br><br><br>

<center><big><b>Remember, you can always ask questions!</b></big>

</center>

<br><br>


---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Recap: Friends of simple linear regression

---

<center>

<div class="imgContainer">
<img src="imgs/model.png" width=70%>
</div>

</center>

---


### Simple linear regression

- Model: $H(x) = w_0 + w_1x$.
- Loss function: squared loss, i.e. $L_\text{sq}(y_i, H(x_i)) = (y_i - H(x_i))^2$.
- Average loss, i.e. empirical risk:

$$R_\text{sq}(w_0, w_1) = \frac{1}{n} \sum_{i = 1}^n \left( y_i - (w_0 + w_1 x_i) \right)^2$$

- Optimal model parameters, found by minimizing empirical risk:

$$w_1^* = \frac{
                        \displaystyle
                        \sum_{i=1}^n (x_i - \bar x)(y_i - \bar y)
                    }{
                        \displaystyle
                        \sum_{i=1}^n (x_i - \bar x)^2
                    } = r \frac{\sigma_y}{\sigma_x}
                    
\qquad\qquad       w_0^* = \bar{y} - w_1^* \bar{x}             
$$

<br>

$$$$

---

### Friends of simple linear regression

- Suppose we use squared loss throughout.
- If our model is $H(x) = w_1x$, it is a **line that is forced through the origin, $(0, 0)$**.
    
    $$w_1^* = \frac{\sum_{i = 1}^n x_iy_i}{\sum_{i = 1}^n x_i^2}$$

- If our model is $H(x) = w_0$, it is a **line that is forced to have a slope of $0$, i.e. a horizontal line**. This is the same as the constant model from before.

$$w_0^* = \text{Mean}(y_1, y_2, ..., y_n)$$

- **Key idea**: $w_0^*$ above is **not** necessarily equal to $w_0^*$ for the simple linear regression model!


---

### Comparing mean squared errors

<div style="width: 100%;">
<div style="width: 65%; float: left"> 
<center>
<!-- <div class="imgContainer"> -->
<img src="imgs/constant-to-slr.png" class=imgContainer width=60%>
</div>

</center>

</div>
    <div style="margin-left: 66%; height: 100px;" markdown="1"> 


$\text{MSE} = \frac{1}{n} \sum_{i = 1}^n \left( y_i - H(x_i) \right)^2$

- The MSE of the best <span style="color:red">simple linear regression model</span> is $\approx 97$.
- The MSE of the best <span style="color:purple">constant model</span> is $\approx 167$.
- The <span style="color:red">simple linear regression model</span> is a more flexible version of the <span style="color:purple">constant model</span>.

</div>
</div>



---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Dot products

---

### Wait... why do we need linear algebra?

- Soon, we'll want to make predictions using more than one feature.
    - Example: Predicting commute times using departure hour and temperature.
- Thinking about linear regression in terms of **matrices and vectors** will allow us to find hypothesis functions that:
    - Use multiple features (input variables).
    - Are non-linear in the features, e.g. $H(x) = w_0 + w_1 x + w_2 x^2$.
- Before we dive in, let's review.

---

### Spans of vectors


- One of the most important ideas you'll need to remember from linear algebra is the concept of the **span** of one or more vectors.
- To jump start our review of linear algebra, let's start by watching [🎥 **this video by <span style="color:blue">3blue</span><span style="color:brown">1brown</span>**](https://youtu.be/k7RM-ot2NWY?si=HRt1nlqIWJPLI0cp).

<center>

<img src="imgs/span.png" width=50%>

</center>

---

### Warning ⚠️

- We're **not** going to cover every single detail from your linear algebra course.
- There will be facts that you're expected to remember that we won't explicitly say.
    - For example, if $A$ and $B$ are two matrices, then $AB \neq BA$.
    - This is the kind of fact that we will only mention explicitly if it's directly relevant to what we're studying.
    - But you still need to know it, and it may come up in homework questions.
- We **will** review the topics that you really need to know well.

---

### Vectors

- A **vector** in $\mathbb{R}^n$ is an **ordered collection of $n$ numbers**.

- We use lower-case letters with an arrow on top to represent vectors, and we usually write vectors as **columns**.

$$\vec{v} = \begin{bmatrix} 8 \\ 3 \\ -2 \\ 5 \end{bmatrix}$$

- Another way of writing the above vector is $\vec{v} = [8, 3, -2, 5]^\intercal$.

- Since $\vec{v}$ has four **components**, we say $\vec{v} \in \mathbb{R}^4$.

---

### The geometric interpretation of a vector

<div style="width: 100%;">
    <div style="margin-left: 35%; height: 100px;" markdown="1"> 

<center>
<img src="imgs/v-only.png" width=35% class=txtContainer>
</center>

</div>
<div style="width: 65%;"> 

- A vector $\vec{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}$ is an arrow to the point $(v_1, v_2, ..., v_n)$ from the origin.

- The **length**, or **$L_2$ norm**, of $\vec{v}$ is: 

$\lVert \vec{v} \rVert = \sqrt{v_1^2 + v_2^2 + ... + v_n^2}$

- A vector is sometimes described as an object with a **magnitude/length** and **direction**.

</div>

</div>

---

### Dot product: coordinate definition

<div style="width: 100%;">
    <div style="margin-left: 65%; height: 100px;" markdown="1"> 

<center>
<img src="imgs/u-and-v.png" class=txtContainer width=40%>
</center>

</div>
<div style="width: 65%; float: left"> 

- The **dot product** of two vectors $\vec{u}$ and $\vec{v}$ in $\mathbb{R}^n$ is written as:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\vec{u} \cdot \vec{v} = \vec{u}^\intercal \vec{v}$

- The computational definition of the dot product:

${\color{purple}\vec{u}} \cdot {\color{#007aff}\vec{v}} = \sum_{i = 1}^n {\color{purple}u_i} {\color{#007aff} v_i} = {\color{purple}u_1} {\color{#007aff} v_1} + {\color{purple}u_2} {\color{#007aff} v_2} + ... + {\color{purple}u_n} {\color{#007aff} v_n}$

- The result is a **scalar**, i.e. a single number.
</div>

</div>

---

<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat.**


Which of these is another expression for the length of $\vec v$?

- A. $\vec v \cdot \vec v$
- B. $\sqrt{\vec v^2}$
- C. $\sqrt{\vec v \cdot \vec v}$
- D. $\vec v^2$
- E. More than one of the above.

---

### Dot product: geometric definition

<div style="width: 100%;">
    <div style="margin-left: 65%; height: 100px;" markdown="1"> 

<center>
<img src="imgs/u-and-v.png" class=txtContainer width=35%>
</center>

</div>
<div style="width: 65%; float: left"> 

- The computational definition of the dot product:

${\color{purple}\vec{u}} \cdot {\color{#007aff}\vec{v}} = \sum_{i = 1}^n {\color{purple}u_i} {\color{#007aff} v_i} = {\color{purple}u_1} {\color{#007aff} v_1} + {\color{purple}u_2} {\color{#007aff} v_2} + ... + {\color{purple}u_n} {\color{#007aff} v_n}$

- The geometric definition of the dot product:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ${\color{purple}\vec{u}} \cdot {\color{#007aff}\vec{v}} = \lVert {\color{purple}\vec{u}} \rVert \lVert {\color{#007aff}\vec{v}} \rVert \cos \theta$

<center>

where $\theta$ is the angle between $\color{purple}\vec{u}$ and ${\color{#007aff}\vec{v}}$.

</center>

- The two definitions are equivalent! This equivalence allows us to find the angle $\theta$ between two vectors.


</div>
</div>

---

<style scoped>section {background: #f2ecf4 }</style>

### Question 🤔
**Answer in chat.**

<div style="width: 100%;">
<div style="margin-left: 50%;" markdown="1"> 

<center>

<img src="imgs/example-theta.png" width=35% class=txtContainer>

</center>

</div>
<div style="width: 45%; float: left"> 

What is the value of $\theta$ in the plot to the right?

</div>

</div>

---

### Orthogonal vectors

- Recall: $\cos 90º = 0$.
- Since $\vec{u} \cdot \vec{v} = \lVert \vec u \rVert \lVert \vec v \rVert \cos \theta$, if the angle between two vectors is $90º$, their dot product is $\lVert \vec u \rVert \lVert \vec v \rVert \cos 90º = 0$.
- If the angle between two vectors is $90º$, we say they are perpendicular, or more generally, **orthogonal**.

- **Key idea**:

$$\boxed{\text{two vectors are \textbf{orthogonal}} \iff \vec{u} \cdot \vec{v} = 0}$$

---

### Exercise

Find a non-zero vector in $\mathbb{R}^3$ orthogonal to:

$$\vec v = \begin{bmatrix} 2 \\ 5 \\ -8 \end{bmatrix}$$

<br>
<br><br><br><br><br><br><br>

---

<style scoped>section {background: linear-gradient(90deg, hsla(290, 25%, 76%, 1) 0%, hsla(284, 80%, 10%, 1) 100%);}</style>

<br><br><br><br><br>

## Spans and projections

---

### Adding and scaling vectors


<div style="width: 100%;">
   <div style="margin-left: 55%; height: 100px;" markdown="1"> 

<center>
<img src="imgs/u-and-v-range.png" class=txtContainer width=40%>
</center>

</div>
<div style="width: 54%; float: left"> 

- The sum of two vectors $\color{purple} \vec u$ and $\color{#007aff}\vec v$ in $\mathbb{R}^n$ is the<br> **element-wise sum** of their components:

${\color{purple}\vec{u}} + {\color{#007aff}\vec{v}} = \begin{bmatrix} {\color{purple} u_1} + {\color{#007aff} v_1} \\ {\color{purple} u_2} + {\color{#007aff} v_2} \\ \vdots \\ {\color{purple} u_n} + {\color{#007aff} v_n} \end{bmatrix}$

- If $c$ is a scalar, then:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $c \vec{v} = \begin{bmatrix} cv_1 \\ cv_2 \\ \vdots \\ cv_n \end{bmatrix}$


</div>
 
</div>




---

### Linear combinations

<div style="width: 100%;">
<div style="width: 50%; float: left"> 

- Let $\vec v_1$, $\vec v_2$, ..., $\vec v_d$ all be vectors in $\mathbb{R}^n$.
- A **linear combination** of $\vec v_1$, $\vec v_2$, ..., $\vec v_d$ is any vector of the form:

$$a_1 \vec v_1 + a_2 \vec v_2 + ... + a_d \vec v_d$$

<center>

where $a_1$, $a_2$, ..., $a_d$ are all scalars.

</center>

</div>
    <div style="margin-left: 65%; height: 100px;" markdown="1"> 

</div>
</div>

<br><br><br><br><br><br><br><br><br><br>


---

### Span

- Let $\vec v_1$, $\vec v_2$, ..., $\vec v_d$ all be vectors in $\mathbb{R}^n$.
- The **span** of $\vec v_1$, $\vec v_2$, ..., $\vec v_d$ is the set of all vectors that can be created using linear combinations of those vectors.
- Formal definition:

$$\text{span}(\vec v_1, \vec v_2, ..., \vec v_d) = \{ a_1 \vec v_1 + a_2 \vec v_2 + ... + a_d \vec v_d : a_1, a_2, ..., a_n \in \mathbb{R} \}$$

<br><br><br><br><br><br><br><br><br><br><br><br>

---

### Exercise

Let $\vec v_1 = \begin{bmatrix} 2 \\ -3 \end{bmatrix}$ and let $\vec v_2 =  \begin{bmatrix} -1 \\ 4 \end{bmatrix}$. Is $\vec y = \begin{bmatrix} 9 \\ 1 \end{bmatrix}$ in $\text{span}(\vec{v_1}, \vec{v_2})$?<br>
If so, write $\vec{y}$ as a linear combination of $\vec{v_1}$ and $\vec{v_2}$.

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

<!-- ---

To whoever teaches this in the future: the slides afterwards were improved in Lecture 7, so you may want to bring some of that material to this lecture. -->

---

### Projecting onto a single vector


<div style="width: 100%;">
    <div style="margin-left: 50%" markdown="1"> 

<img src="imgs/project-y-on-x.png" width=40%% class=txtContainer>

</div>
<div style="width: 45%; float: left"> 

- Let $\color{#007aff} \vec x$ and $\color{orange} \vec y$ be two vectors in $\mathbb{R}^n$.
- The span of $\color{#007aff} \vec x$ is the set of all vectors of the form:

$w \color{#007aff} \vec x$

<center>

where $w \in \mathbb{R}$ is a scalar.

</center>

- **Question**: What vector in $\text{span}({\color{#007aff}\vec x})$ is closest to $\color{orange} \vec y$?

- The vector in $\text{span}({\color{#007aff}\vec x})$ that is closest to $\color{orange} \vec y$ is the **projection of $\color{orange} \vec y$ onto $\text{span}({\color{#007aff}\vec x})$**.

</div>

</div>

---

### Projection error

<div style="width: 100%;">
    <div style="margin-left: 50%" markdown="1"> 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="imgs/project-y-on-x.png" class=txtContainer width=45%%>

</div>
<div style="width: 50%; float: left"> 

- Let ${\color{red} \vec e} = {\color{orange} \vec y} - w{\color{#007aff} \vec x}$ be the **projection error**: that is, the vector that connects ${\color{orange} \vec y}$ to $\text{span}({\color{#007aff}\vec x})$.
- **Goal**: **Find the $w$ that makes $\color{red} \vec e$ as short as possible**.
    - That is, minimize:
    $\lVert {\color{red} \vec e} \rVert$
    - Equivalently, minimize:
    $\lVert {\color{orange} \vec y} - w {\color{#007aff} \vec{x}} \rVert$
- **Idea**: To make $\color{red} \vec{e}$ has short as possible, it should be **orthogonal to $w{\color{#007aff}\vec x}$**.

</div>

</div>

---

### Minimizing projection error

- **Goal**: **Find the $w$ that makes ${\color{red} \vec e} = {\color{orange} \vec y} - w {\color{#007aff} \vec{x}}$ as short as possible**.

- **Idea**: To make $\color{red} \vec{e}$ as short as possible, it should be **orthogonal to $w{\color{#007aff}\vec x}$**.

- Can we prove that making $\color{red} \vec{e}$ orthogonal to $w{\color{#007aff}\vec x}$ minimizes $\lVert {\color{red} \vec{e}} \rVert$?

<br><br><br><br><br><br><br><br><br>

---

### Minimizing projection error

- **Goal**: **Find the $w$ that makes ${\color{red} \vec e} = {\color{orange} \vec y} - w {\color{#007aff} \vec{x}}$ as short as possible**.
- Now we know that to minimize $\lVert {\color{red} \vec{e}} \rVert$,  $\color{red} \vec{e}$ must be orthogonal to $w{\color{#007aff}\vec x}$.
- Given this fact, how can we solve for $w$?

<br><br><br><br><br><br><br><br><br>

---

### Orthogonal projection

- **Question**: What vector in $\text{span}({\color{#007aff}\vec x})$ is closest to $\color{orange} \vec y$?

- **Answer**: It is the vector $w^* {\color{#007aff}\vec x}$, where:

$$w^* = \frac{{\color{#007aff}\vec x} \cdot {\color{orange}\vec y}}{{\color{#007aff}\vec x} \cdot {\color{#007aff}\vec x}}$$

- Note that $w^*$ is the solution to a minimization problem, specifically, this one:

$${\color{red} \text{error}}(w) = \lVert {\color{red} \vec{e}} \rVert = \lVert {\color{orange} \vec y} - w {\color{#007aff} \vec{x}} \rVert $$

- We call $w^* {\color{#007aff} \vec{x}}$ the **orthogonal projection of $\color{orange} \vec{y}$ onto $\text{span}({\color{#007aff} \vec{x}})$**.
    - Think of $w^* {\color{#007aff} \vec{x}}$ as the "shadow" of $\color{orange} \vec{y}$.

---

### Exercise

Let $\vec{a} = \begin{bmatrix} 5 \\ 2 \end{bmatrix}$ and $\vec{b} = \begin{bmatrix} -1 \\ 9 \end{bmatrix}$.

What is the orthogonal projection of $\vec{a}$ onto $\text{span}(\vec b)$?<br> Your answer should be of the form $w^* \vec b$, where $w^*$ is a scalar.


<br><br><br><br><br><br><br><br><br>

---

### Moving to multiple dimensions

- Let's now consider three vectors, $\color{orange}\vec{y}$, $\color{#007aff} \vec{x}^{(1)}$, and $\color{#007aff} \vec{x}^{(2)}$, all in $\mathbb{R}^n$.

- **Question**: What vector in $\text{span}({\color{#007aff} \vec{x}^{(1)}}, {\color{#007aff} \vec{x}^{(2)}})$ is closest to ${\color{orange} \vec{y}}$? 

    - Vectors in $\text{span}({\color{#007aff} \vec{x}^{(1)}}, {\color{#007aff} \vec{x}^{(2)}})$ are of the form $w_1 {\color{#007aff} \vec{x}^{(1)}} + w_2 {\color{#007aff} \vec{x}^{(2)}}$, where $w_1$, $w_2 \in \mathbb{R}$ are scalars.

- Before trying to answer, let's watch [**🎥 this animation that Jack, one of our tutors, made**](https://youtu.be/dJcbJKpYywk?si=giWFps-ixYDXBwzh).

<center>

<img src="imgs/jack.png" width=38%>

</center>

---

### Minimizing projection error in multiple dimensions

- **Question**: What vector in $\text{span}({\color{#007aff} \vec{x}^{(1)}}, {\color{#007aff} \vec{x}^{(2)}})$ is closest to ${\color{orange} \vec{y}}$? 
    - That is, what vector minimizes $\lVert {\color{red} \vec{e}} \rVert$, where:

    $${\color{red} \vec{e}} = {\color{orange} \vec{y}} - w_1 {\color{#007aff} \vec{x}^{(1)}} - w_2 {\color{#007aff} \vec{x}^{(2)}}$$

- **Answer**: It's the vector such that $w_1 {\color{#007aff} \vec{x}^{(1)}} + w_2 {\color{#007aff} \vec{x}^{(2)}}$ is **orthogonal** to $\color{red} \vec e$.

- **Issue**: Solving for $w_1$ and $w_2$ in the following equation is difficult:

$$\left( w_1 {\color{#007aff} \vec{x}^{(1)}} + w_2 {\color{#007aff} \vec{x}^{(2)}} \right) \cdot \underbrace{\left( {\color{orange} \vec y} -   w_1 {\color{#007aff} \vec{x}^{(1)}} - w_2 {\color{#007aff} \vec{x}^{(2)}}\right)}_{\color{red} \vec e} = 0$$

---

### What's next?

- It's hard for us to solve for $w_1$ and $w_2$ in:

$$\left( w_1 {\color{#007aff} \vec{x}^{(1)}} + w_2 {\color{#007aff} \vec{x}^{(2)}} \right) \cdot \underbrace{\left( {\color{orange} \vec y} -   w_1 {\color{#007aff} \vec{x}^{(1)}} - w_2 {\color{#007aff} \vec{x}^{(2)}}\right)}_{\color{red} \vec e} = 0$$


- **Solution**: Combine ${\color{#007aff} \vec{x}^{(1)}}$ and ${\color{#007aff} \vec{x}^{(2)}}$ into a single **matrix**, ${\color{#007aff} X}$, and express $w_1 {\color{#007aff} \vec{x}^{(1)}} + w_2 {\color{#007aff} \vec{x}^{(2)}}$ as a **matrix-vector multiplication**, ${\color{#007aff} X}\vec{w}$.

- **Next time**: Formulate linear regression in terms of matrices and vectors!