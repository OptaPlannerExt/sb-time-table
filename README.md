# Spring Boot Optaplanner Time Table Demo

<h1>OptaPlanner Workshop - School Timetabling Problem</h1>
<ul>
<li><a href="#background">Background</a></li>
<li><a href="#prerequisites">Prerequisites</a></li>
<li><a href="#goal">Goals and learning objectives</a></li>
<li><a href="#planningProblem">What is a Planning Problem?</a></li>
<li><a href="#useCase">Use Case</a></li>
<li><a href="#architecture">Architecture</a></li>
<li><a href="#labMaterial">Lab Material</a></li>
<li><a href="#environmentUsageInst">Environment Usage Instructions</a></li>
<ul>
<li><a href="#codeReadyWS">CodeReady WorkSpaces</a></li>
<li><a href="#aCRWS">Accessing CodeReady Workspaces</a></li>
<li><a href="#createWS">Creating the Workspace</a></li>
</ul>
</li>
<li><a href="#solution">Solution</a><br />
<ul>
<li><a href="#domainModel">Domain Model</a></li>
<li><a href="#defConstScore">Define the constraints and calculate the score</a></li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<h2 id="background"><span style="color: #000000;">Background</span></h2>
<div>
<p>Almost every organization has to deal with planning problems to perform its business operations. Whether its transporting goods from a warehouse to stores, assigning employees to shifts, inspecting and servicing vehicles, or allocating tasks to departments, the more efficient and effective these tasks can be performed, the bigger the competitive advantage of an organization will be.</p>
</div>
<div>
<p>The aforementioned planning problems fall in a category of problems that are, in the academic world, known as NP-Complete problems. What this in essence means is that there is no known algorithm to solve these problems in polynomial time. Whenever the problem space of a problem grows, the time to find the solution to these problems grows exponentially.</p>
<p>Universities and schools aim to provide high quality lesson schedules to their teachers and students. Depending on room capacity and availability,&nbsp;<em>school timetabling</em>&nbsp;decides when and where a lecture takes place. Optimizing this planning problem with a constraint solver AI improves teacher and student satisfaction by reducing commute and gap hours while adhering to other constraints such as conflicting lessons, teacher availability, room availability and room capacity.</p>
<p><img src="https://github.com/OptaPlannerExt/sb-time-table/blob/master/images/schoolTimetablingInputOutput.png" alt="" width="800" height="600" /></p>
<p>&nbsp;</p>
<p>In this lab you will be introduced to OptaPlanner/Business Optimizer, Red Hat&rsquo;s A.I. Constraint Solver. OptaPlanner provides advanced A.I. heuristic algorithms that, combined with a domain model and constraint rules, allows organizations to find optimal solutions to their planning problems.</p>
<h2>&nbsp;</h2>
<h2 id="prerequisites"><span style="color: #000000;">Prerequisites</span></h2>
<div>
<p><strong>Tools</strong></p>
</div>
<div>
<p>Most of the tools required will be provided in your by CodeReady Workspaces environment. The tools you need locally are:</p>
</div>
<div>
<ul>
<li>
<p>a browser (Chrome, Firefox)</p>
</li>
</ul>
</div>
<div>
<p><strong>Skills</strong></p>
</div>
<div>
<ul>
<li>
<p>Familiarity with IDEs, like Eclipse, IntelliJ, Visual Studio Code, CodeReady Workspaces.</p>
</li>
<li>
<p>Java</p>
</li>
<li>
<p>Maven</p>
</li>
</ul>
<p>&nbsp;</p>
<h2 id="goal"><span style="color: #000000;">Goals</span></h2>
<div>
<ul>
<li>
<p>Understand what planning problems are.</p>
</li>
<li>
<p>Understand how OptaPlanner can provide optimal solutions to these kind of problems using advanced A.I. Heuristic Algorithms.</p>
</li>
<li>
<p>Annotate an OptaPlanner domain model for the&nbsp;<em>School Timetabling</em>&nbsp;problem.</p>
</li>
<li>
<p>Write constraint rules for the&nbsp;<em>School Timetabling</em>&nbsp;problem.</p>
</li>
<li>
<p>Understand the OptaPlanner API.</p>
</li>
</ul>
<p>&nbsp;</p>
<h2 id="planningProblem"><span style="color: #000000;">What is a Planning Problem?</span></h2>
<p><img src="https://github.com/OptaPlannerExt/sb-time-table/blob/master/images/whatIsAPlanningProblem.png" alt="" />&nbsp;</p>
<div>
<p>Examples of known planning problems are:</p>
</div>
<div>
<ul>
<li>
<p>Vehicle Routing (VRP)</p>
</li>
<li>
<p>Shift Assignment</p>
</li>
<li>
<p>Employee Rostering</p>
</li>
<li>
<p>Conference Scheduling</p>
</li>
<li>
<p>Hospital Bed Planning</p>
</li>
<li>
<p>Exam Scheduling</p>
</li>
</ul>
</div>
<p>Examples of known planning problems are: Vehicle Routing (VRP) Shift Assignment Employee Rostering Conference Scheduling Hospital Bed Planning Exam Scheduling In all of these examples, there is a certain goal, e.g. "reduce fuel costs". There are resources, e.g. "vehicles". And there are constraints, e.g. "the maximum vehicle capacity". OptaPlanner takes all these facts into account and will use A.I. heuritics algorithms to find the optimal solution to a given problem. Constraints, their level, and their weights, can be easily altered, giving the user full flexibility on what he/she wants to optimize on. For example, purely optimizing on costs can have negative consequences in other areas, like employee well-being or CO2 emissions. By providing the ability to users to configure the area in which they want to optimize, OptaPlanner gives users full flexibility and control over their use-case.</p>
<p>&nbsp;</p>
<h2 id="useCase">Use Case</h2>
<p>&nbsp;</p>
<div class="paragraph">
<p>Your service will assign&nbsp;<code>Lesson</code>&nbsp;instances to&nbsp;<code>Timeslot</code>&nbsp;and&nbsp;<code>Room</code>&nbsp;instances automatically by using AI to adhere to hard and soft scheduling&nbsp;<em>constraints</em>, such as:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>A room can have at most one lesson at the same time.</p>
</li>
<li>
<p>A teacher can teach at most one lesson at the same time.</p>
</li>
<li>
<p>A student can attend at most one lesson at the same time.</p>
</li>
<li>
<p>A teacher prefers to teach in a single room.</p>
</li>
<li>
<p>A teacher prefers to teach sequential lessons and dislikes gaps between lessons.</p>
</li>
<li>
<p>A student dislikes sequential lessons on the same subject.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>Mathematically speaking, school timetabling is an&nbsp;<em>NP-hard</em>&nbsp;problem. That means it is difficult to scale. Simply brute force iterating through all possible combinations takes millions of years for a non-trivial dataset, even on a supercomputer. Luckily, AI constraint solvers such as OptaPlanner have advanced algorithms that deliver a near-optimal solution in a reasonable amount of time.</p>
</div>
<p><img src="https://github.com/OptaPlannerExt/sb-time-table/blob/master/images/optaplanner-time-table.png" alt="" /></p>
<h2 id="architecture">Architecture</h2>
<div>
<p>OptaPlanner is a lightweight Java library, and as such can be deployed on, and integrated with, virtually any environment.</p>
<p><img src="https://github.com/OptaPlannerExt/sb-time-table/blob/master/images/spring-boot.png" alt="" /></p>
<p>OptaPlanner solutions can be run on premise and in the cloud, as nightly (batch) planning engine, as well as realtime planning engine. This flexibility allows users to architect, design and implement an OptaPlanner solution specifically for their problem, their needs and their environment. As OptaPlanner is a Java-based platform, OptaPlanner solutions are, obviously, built in Java. This means that the problem&rsquo;s domain model can be defined and written as simple PoJo, and constraint rules can be written in either Java or in Drools, the open-source business rules engine.</p>
<p>In this lab we will simply use OptaPlanner as a library runnning on Spring Boot. Data will be loaded from the code, and persisted in the database. We will build and run the planning engine from CodeReady Workspace.</p>
<p>&nbsp;</p>
<h2 id="labMaterial">Lab Material</h2>
<div>
<p>The lab material and the instructions are hosted on GitHub, at the following URL:</p>
</div>
<div>
<p><a href="https://github.com/OptaPlannerExt/sb-time-table">https://github.com/OptaPlannerExt/sb-time-table</a></p>
</div>
<div>&nbsp;</div>
<div>
<p>The scheduled time for this lab is 90 minutes and therefore some parts of the application have already been pre-defined for you.</p>
<p>&nbsp;</p>
<h2 id="environmentUsageInst">Environment Usage Instructions</h2>
<div>
<h2 id="codeReadyWS">CodeReady WorkSpaces</h2>
<div>
<div>
<p>Red Hat CodeReady Workspaces is a developer workspace server and cloud IDE. Workspaces are defined as project code files and all of their dependencies neccessary to edit, build, run, and debug them. Each workspace has its own private IDE hosted within it. The IDE is accessible through a browser. The browser downloads the IDE as a single-page web application.</p>
</div>
<div>
<p>Red Hat CodeReady Workspaces provides:</p>
</div>
<div>
<ul>
<li>
<p>Workspaces that include runtimes and IDEs</p>
</li>
<li>
<p>RESTful workspace server</p>
</li>
<li>
<p>A browser-based IDE</p>
</li>
<li>
<p>Plugins for languages, framework, and tools</p>
</li>
<li>
<p>An SDK for creating plugins and assemblies</p>
</li>
</ul>
</div>
<div>
<table>
<tbody>
<tr>
<td>
<div>Note</div>
</td>
<td>The CodeReady Workspaces environment has been provisioned for you so that can run the labs in a pre-provisioned environment. However, you can also run the labs on your own laptop, provided that you have an IDE, JDK 8+, Maven and Git tools installed. There is no dependency on CodeReady Workspaces and/or OpenShift. All lab materials are provided as standard Maven projects on GitHub.</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div>
<h2 id="aCRWS">Accessing CodeReady Workspaces</h2>
<div>
<div>
<p>A CodeReady Workspaces environment has been created for every workshop user. To access your environment, use the link that is provided to you by your lab instructor. You can login with the OpenShift username and password that have been provided to you.</p>
</div>
<div>
<ul>
<li>
<p>In the CodeReady Workspaces login screen, login with the workshop credentials that have been provided to you.</p>
<div>
<p><a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-login-openshift.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-login-openshift.png" alt="codeready login openshift" /></a></p>
</div>
<div>
<p><a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-login-openshift-username-password.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-login-openshift-username-password.png" alt="codeready login openshift username password" /></a></p>
</div>
</li>
<li>
<p>An&nbsp;<em>Authorize Access</em>&nbsp;screen will be presented. Leave&nbsp;<code>user_full</code>&nbsp;checkbox checked and click on&nbsp;<code>Allow selected permissions</code>.&nbsp;<a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-authorize-access.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-authorize-access.png" alt="codeready authorize access" /></a></p>
</li>
<li>
<p>In the next screen, provide additional user information. This can be dummy information for this workshop.&nbsp;<a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-user-information.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-user-information.png" alt="codeready user information" /></a></p>
</li>
</ul>
</div>
<div>
<p>CodeReady Workspaces will open and show the initial screen.</p>
</div>
<div>
<p><a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-initial-screen.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-initial-screen.png" alt="codeready initial screen" /></a></p>
</div>
<div>
<h3 id="createWS">Creating the Workspace</h3>
<div>
<p>We will now create our workspace from a the&nbsp;<code>OptaPlanner Workshop</code>&nbsp;stack that we&rsquo;ve provided in your CodeReady Workspaces environment. This stack contains: * OpenJDK 8 * Maven 3.5.x * The OptaPlanner Timetable lab repository * Pre-defined Maven commands to build the project, run its unit-tests and run the OptaPlanner Benchmarker.</p>
</div>
<div>
<p>To create a new workspace, scroll down in the&nbsp;<code>Getting Started with CodeReady Workspaces</code>&nbsp;page until you see the&nbsp;<code>OptaPlanner Workshop</code>&nbsp;stack. Select the stack and click on the green&nbsp;<code>CREATE &amp; OPEN</code>&nbsp;button.</p>
</div>
<div>
<p><a href="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/blob/master/modules/01_optaplanner/images/codeready-optaplanner-workshop-stack.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/RedHat-Middleware-Workshops/optaplanner-2-hour-workshop/raw/master/modules/01_optaplanner/images/codeready-optaplanner-workshop-stack.png" alt="codeready optaplanner workshop stack" /></a></p>
</div>
<div>
<p>CodeReady Workspaces will create a new workspace container in our OpenShift instance. Note that creating the container can take some time. Wait for this process to finish.</p>
</div>
<div>
<p>The workspace automatically import the base project, which already includes the solution&rsquo;s domain classed, into CodeReady. This project is automatically imported from Github, as defined in our&nbsp;<em>devfile</em>.</p>
</div>
<div>
<p>When your environment is up and running, click on the&nbsp;<em>Explorer</em>&nbsp;button in the upper left corner to open the project explorer. Expand the&nbsp;<code>sb-time-table</code>&nbsp;and navigate the project structure.</p>
</div>
<div>&nbsp;&nbsp;</div>
<div>
<h2 id="solution">Solution</h2>  
<h2 id="domainModel">Domain Model</h2>
<p><img src="https://github.com/OptaPlannerExt/sb-time-table/blob/master/images/optaplanner-time-table-class-diagram-annotated.png" alt="" /></p>
<p class="p1">&nbsp;</p>
<p class="p2"><span class="s1"><strong>Time Slot</strong> - The Timeslot class represents a time interval when lessons are taught, for example, Monday 10:30 - 11:30 or Tuesday 13:30 - 14:30. For simplicity&rsquo;s sake, all time slots have the same duration and there are no time slots during lunch or other breaks.</span></p>
<p class="p2">&nbsp;</p>
<p class="p2"><span class="s1"><strong>Room</strong> - The&nbsp;Room&nbsp;class represents a location where lessons are taught, for example,&nbsp;Room A&nbsp;or&nbsp;Room B. For simplicity&rsquo;s sake, all rooms are without capacity limits and they can accommodate all lessons.</span></p>
<p class="p2"><span class="s1">&nbsp;</span></p>
<p class="p2"><span class="s1"><strong>Lesson</strong> - During a lesson, represented by the&nbsp;Lesson&nbsp;class, a teacher teaches a subject to a group of students, for example,&nbsp;Math by A.Turing for 9th grade&nbsp;or&nbsp;Chemistry by M.Curie for 10th grade. If a subject is taught multiple times per week by the same teacher to the same student group, there are multiple&nbsp;Lesson&nbsp;instances that are only distinguishable by&nbsp;id. For example, the 9th grade has six math lessons a week.</span></p>
<p class="p2"><span class="s1">During solving, OptaPlanner changes the&nbsp;timeslot&nbsp;and&nbsp;room&nbsp;fields of the&nbsp;Lesson&nbsp;class, to assign each lesson to a time slot and a room. Because OptaPlanner changes these fields,&nbsp;Lesson&nbsp;is a&nbsp;<em>planning entity.</em></span></p>
<p>&nbsp;</p>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
