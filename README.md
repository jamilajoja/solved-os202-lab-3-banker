Download Link: https://assignmentchef.com/product/solved-os202-lab-3-banker
<br>
<em>                                                                                                                                                                            </em>

The goal of this lab is to do resource allocation using both an optimistic resource manager and the banker’s algorithm of Dijkstra. The optimistic resource manager is simple: Satisfy a request if possible, if not make the task wait; when a release occurs, try to satisfy pending requests in a FIFO manner.

Your program takes one command line argument, the name of the file containing the input. After reading (all) the input, the program performs two simulations: one with the optimistic manager and one with the banker. Output is written to stdout (the screen). Input files are available on NYU Classes, together with the expected output. To help in debugging there are also cycle by cycle results included for a few of the examples in the “detailed” files. You are <strong>NOT </strong>required to produce these “detailed” outputs; they are for your benefit only. We may test your program on additional input as well.

The input begins with two values T, the number of tasks, and R, the number of resource types, followed by R additional values, the number of units present of each resource type. (If you set “arbitrary limits” on say T or R, you must document this in your readme, check that the input satisfies the limits, print an error if it does not, and set the limits high enough so that the required inputs all pass.) Then come multiple inputs, each representing the next activity of a specific task. The possible activities are initiate, request, release, and terminate. Time is measured in fixed units called cycles and, for simplicity, no fractional cycles are used. The manager can process one activity (initiate, request, or release) for each task in one cycle. However, the terminate activity does <strong>not </strong>require a cycle.

To ease the programming, I have specified all activities to have the same format, namely a string followed by four unsigned integers.

The initiate activity, which must precede all others for that task, is written initiate task-number delay resource-type initial-claim

(The optimistic manager ignores the claim.) If there are R resource types, there are R initiate activities for each task, each requiring one cycle. The delay value is not used for initiate; I include it so that all activities have the same format.

The request and release activities are written

request         task-number delay resource-type number-requested release  task-number delay resource-type number-released

The delay value represents the number of cycles between the completion of the previous activity for this process and the beginning of the current activity. For example, assume the previous activity was a request and the current activity is a release with a delay of 6. Then, after the request is satisfied, the process is delayed 6 cycles before making the release (presumably the task was computing during these 6 cycles, but that is not relevant to the manager and hence not relevant for the lab). The process retains its resources during the delay.

Finally the terminate operation, which does <strong>not </strong>require a cycle is written terminate task-number delay unused unused

The last two values are not used; I include them so that all activities have the same format.

<h1>Commenting your program</h1>

You must include enough high-level comments in your program so that a reader (e.g., the grader) who is expert in the programming language you use and knowledgeable about resource management can understand the basic operation of your program. For example, you should make clear when you are checking for safety, when you are checking for deadlock, when you are releasing a previously blocked task, etc. You must also supply comments for your major data structures.

Each function/procedure/method you write must have an introductory comment stating what the procedure does and how it does it (at a high level).

Note that these <strong>required </strong>comments will form a (small) portion of the grade for this lab.

<h1>Input format</h1>

As in lab 1 (linker) we use free form input. Hence a single activity may span several lines and (parts of) several activities may be on one line. For this lab, I advise you to read all the data before processing. The data for each task occurs in order, but the tasks themselves may be interspersed. Input set 1, on the web and reprinted below, has all of task one followed by all of task two. Input set 8, on the web, represents the same run but the lines for tasks one and two are interleaved.

<em>OS 202-02 2018-19 Spring Lab 3 Banker                                                                                                                             Page </em>2

<h1>Running your program</h1>

Your program must read its input from a file, whose name is given as a command line argument. The format of the input is shown above; sample inputs are on the web.

<h1>Output</h1>

At the end of the run, print, for each task, the time taken, the waiting time, and the percentage of time spent waiting. Also print the total time for all tasks, the total waiting time, and the overall percentage of time spent waiting.

<h1>Error Checks</h1>

When implementing banker’s algorithm, if a task’s initial claim exceeds the resources present or if, during execution, a task’s requests exceed its claims, print an informative message, abort the task, and release all its resources. This does not apply to the optimistic allocator since it does not have the concept of initial claim.

<h1>Deadlock</h1>

Deadlock cannot occur for banker’s algorithm, but can for the optimistic resource manager. If deadlock is detected, print a message and abort the lowest numbered deadlocked task after releasing all its resources. If deadlock remains, print another message and abort the next lowest numbered deadlocked task, etc.

We learned sophisticated algorithms for detecting deadlock. You are <strong>not </strong>expected to implement one of these. Instead you should use the following simple algorithm that detects a deadlock when all non-terminated tasks have outstanding requests that the manager cannot satisfy. Note that, if a deadlock actually occurs during cycle n, you may not detect it until much later since there may be non-deadlocked processes running. If you detect the deadlock at cycle k, you abort the task(s) at cycle k and hence its/their resources become available at cycle k+1. This simple deadlock detection algorithm is not used in practice.

<h1>Important Note</h1>

Items returned at time n are not available until time n+1, For example, if task 1 returns 10 units during cycle 6-7 and task 3 requests 10 units during that same cycle, the 10 units returned by task 1 are <strong>not </strong>available for the optimistic manager to give out and, when the banker makes its safety check, these 10 units are <strong>not </strong>considered available. They become available at time 7 for use in cycle 7-8.

<h1>The first data set</h1>

The input for the first run is.

2 1 4 initiate 1 0 1 4 request              1 0 1 1 release         1 0 1 1 terminate 1 0 0 0 initiate 2 0 1 4 request              2 0 1 1 release         2 0 1 1 terminate 2 0 0 0

The first line asserts that this run has 2 tasks and 1 resource type with 4 units.

The next line indicates that the run begins (at cycle 0-1, the cycle starting at 0 and ending at 1) with task 1

claiming (all) 4 units of resource 1. Further down on line 6 we see that task 2 also claims 4 units of resource 1 during cycle 0-1.

From lines 3 and 7 we learn that each task requests a unit during cycle 1-2 and returns that unit during the next cycle after the request is granted. For the optimistic manager, the request is granted at 2 (the end of cycle 1-2) and the resource is returned during 2-3.

Input 1 has all delays zero. For the optimistic manager each task terminates at time 3.