
# Queue management <> Product Development

<img alt="coffee shop" src="./coffee.png" width="300px" />

Imagine you're at your favorite coffee shop. You walk in, and there's a line of people waiting to order (the queue). The barista takes orders and makes coffee (the service). More people keep coming in (arrivals). The speed at which the barista works determines how fast the line moves and how quickly people get their coffee (the service rate). The rate at which people enter the shop is the arrival rate. The number of people in line is the queue length, and the time you spend waiting is your waiting time. The overall rate at which finished coffees are handed out is the throughput.

Queue theory is the mathematical study of these kinds of waiting lines. It gives us a way to understand and predict how systems behave when resources are limited and demand is not always predictable.

Now, how does this apply to software project management? Think of your team's workflow like that coffee shop:

- **Work Items (Tasks, Stories, Bugs):** These are your "customers" arriving at the system. The rate at which new work comes in is your **arrival rate**.
- **The Team (Developers, Testers, DevOps, etc.):** These are your "servers" providing the service (completing the work). The rate at which your team can complete work items is your **service rate** (or capacity).
- **Work in Progress (WIP):** This is the "queue length" – the number of tasks that have been started but aren't yet finished and delivered. This includes tasks sitting in backlogs, in development, in testing, etc.
- **Cycle Time:** This is the "waiting time" – the total time a work item spends in the system, from when it's first considered "in progress" to when it's completed and delivered.

Here's where queue theory provides powerful insights for project management:

1. **Understanding the Relationship between WIP, Cycle Time, and Throughput (Little's Law):** One of the most fundamental principles in queue theory is **Little's Law**. In the context of project management, it can be simplified to:

   _Average WIP = Average Throughput × Average Cycle Time_

   This is huge! It tells you that for a stable system, there's a direct relationship between how much work you have in progress, how fast you're finishing it (throughput), and how long each item takes to get done (cycle time).

   - If you have a high amount of WIP, even with a decent throughput, your cycle times will be long. (Imagine a massive line at the coffee shop, even with a fast barista, you'll still wait a while).
   - If you want to reduce cycle time (get features to customers faster), you need to either increase throughput (finish work faster) or, more often and more effectively, reduce WIP.

2. **Identifying Bottlenecks:** Just like a slow barista can create a bottleneck at the coffee shop, a stage in your development pipeline where work piles up indicates a bottleneck. Queue theory helps you see where the queues are forming and growing, pointing directly to the constrained part of your process.

3. **Predicting and Managing Flow:** By understanding your arrival rate and service rate, you can start to predict how changes in one will affect the others. If the arrival rate of new features suddenly spikes, and your service rate stays the same, queue theory tells you that your WIP will likely increase and your cycle times will get longer. This allows you to proactively manage expectations or consider increasing capacity.

4. **Optimizing Resource Allocation:** Queue theory helps you think about the impact of adding more "servers" (developers, testers). Adding more people might seem like the obvious solution to speed things up, but queue theory can help analyze if the bottleneck is truly a lack of capacity or something else, like too much WIP or a handoff issue between stages.

5. **Improving Throughput:** Ultimately, project management aims to maximize throughput – delivering valuable software features consistently and quickly. By applying queue theory principles, you can focus on strategies that directly impact the elements of the queueing system:

   - **Limit WIP:** This is a core tenet of many agile methodologies like Kanban, and Little's Law shows why. Reducing WIP directly reduces cycle time for the same throughput. Less context switching, clearer priorities, and faster feedback loops.
   - **Increase Service Rate (Carefully):** This could involve improving team skills, optimizing processes, reducing technical debt, or adding resources *at the bottleneck*.
   - **Manage Arrival Rate:** While you can't always control when requests come in, you can control which ones are pulled into the system (prioritization).

In essence, queue theory provides a lens through which to view your software development process as a system with work flowing through it. By understanding the dynamics of arrivals, service, and waiting, you can make more informed decisions about how to manage your team's workload, identify and address inefficiencies, and ultimately improve the speed and predictability of your delivery – your throughput. It helps you move from a reactive approach to a more proactive and data-driven one.

## Why a single queue is faster than multiple queues

Imagine your project's backlog and your development team.

**Scenario 1: Five Queues, One Server Each**

Think of this like having five different specialized sub-teams (or even individuals) and five separate backlogs (queues) for them. Each sub-team/individual pulls work *only* from their specific backlog.

- **Diagram 1: Multiple Queues**

  ```
  +---------+     +---------+          +---------+     +---------+
  | Queue 1 | --> | Server 1|     ...  | Queue 5 | --> | Server 5|
  +---------+     +---------+          +---------+     +---------+
      |               |                    |               |
      |               |                    |               |
      v               v                    v               v
  Arrivals 1    Completed Work 1  ...   Arrivals 5    Completed Work 5

  ```

- **The Problem:** Work arrives unevenly. Sometimes, Queue 1 gets flooded with high-priority tasks, while Queue 3 is nearly empty. Server 1 becomes a bottleneck, with tasks piling up in Queue 1 and long waiting times. Meanwhile, Server 3 might be idle or underutilized because its specific queue has no work, even though there's plenty of work *overall* in the system (just in other queues).

**Scenario 2: Single Queue, Five Servers**

Think of this like having one shared team (five developers) pulling tasks from a single, prioritized backlog. Any available developer can pick up the next ready task, regardless of its original "type" or "source" (assuming the team is generalist enough).

- **Diagram 2: Single Queue**

  ```
  +---------+     +---------+
  |         | --> | Server 1|
  |         |     +---------+
  |         |     +---------+
  | Single  | --> | Server 2|
  |  Queue  |     +---------+
  |         |         ...
  |         |     +---------+
  |         | --> | Server 5|
  +---------+     +---------+
      |               |
      |               |
      v               v
   Arrivals     Completed Work (aggregated)

  ```

- **The Benefit:** Work arrives into a single pool. When a server (developer) finishes a task, they simply go to the single queue and pull the *next* available task. This effectively pools the serving capacity.

[Demo](https://aheckmann.github.io/queue-demos/)

**Why the Single Queue System Has Higher Throughput (and Lower Waiting Times)**

The key reason lies in how the system handles **variability**. Real-world task arrivals are bursts, and service times (how long a task takes) vary.

1. **Reduced Idleness:** In the multiple queue system, a server can be idle even if there's work waiting *in another queue*. In the single queue system, a server is only idle if there is *absolutely no work* left in the entire system backlog. Pooling the servers against a single queue maximizes the chances that a server is busy whenever there is work to be done. This higher effective utilization of your *total* team capacity is crucial.

2. **Better Load Balancing:** The single queue inherently load balances the work across the available servers. The next item is served by the next *available* server, not a server tied to a specific queue that might be overloaded or empty.

3. **Lower Waiting Times:** Because servers are less likely to be idle when there's work, tasks spend less time sitting in a queue waiting for *a specific* server to become free. They just need *any* server from the pool.

**Connecting to Math (Intuition with Little's Law)**

Remember Little's Law:

$`L=λW`$

Where:

- *L* = Average Work in Progress (WIP) (items in the system/queue)
- *λ* = Average Arrival Rate (how fast work comes in)
- *W* = Average Cycle Time (time an item spends in the system/queue)

Rearranging, we get: $`W=L/λ`$

And since Throughput is ideally equal to the Arrival Rate (λ) in a stable system where work isn't infinitely piling up, we can also relate *W* to *L* and Throughput.

While the *theoretical maximum* throughput (when every server is 100% busy with no waiting) might be the same in both systems (5 servers \* average service rate), real-world systems have variability and can rarely sustain 100% utilization without infinite queues.

Queueing theory shows that for the same total arrival rate (*λ*) and the same total service capacity (5 servers), a single queue system will have a significantly lower average waiting time (*W*). Since *W=L/λ*, a lower *W* means a lower *L* (WIP) for the same arrival rate.

Why does lower *W* imply higher throughput *in practice*? Because work flows more smoothly. Items don't get stuck behind a traffic jam in one specific lane while other lanes are empty. The work progresses through the system more consistently. This consistency and reduced blockage mean that over any significant period, more items are likely to *complete* in the single-queue system compared to the multiple-queue system suffering from uneven loading and idle servers.

Think of it like checkout lines at a grocery store: having one long line feeding multiple cashiers (single queue, multiple servers) is much faster and fairer than having a separate line for each cashier (multiple queues, single server each). You rarely pick the "wrong" line and wait forever while others are empty.

**In Summary:**

A single queue with multiple servers handles the inherent variability of work arrivals and service times much more efficiently than multiple independent queues. By pooling the serving capacity, it reduces server idleness, improves load balancing, drastically cuts down average waiting time (W), lowers Work in Progress (L), and results in higher *effective* throughput under realistic conditions.

Applying this knowledge to product development, we should aim to have as few task queues as necessary and as many generalist developers as possible.

## Server Utilization

Let's talk about server utilization in the context of queue theory and why aiming for something like 80% is often considered a sweet spot, rather than pushing for 100%.

First, let's define **Utilization (ρ)**. In queue theory terms, it's the proportion of time a server (or a group of servers) is busy serving customers (processing work items). It's calculated as:

_ρ = Arrival Rate / Service Rate​_

Or, if you have multiple identical servers (c):

_ρ = Arrival Rate / c × Service Rate per server​_

Utilization tells you how busy your team or system is. 100% utilization means your team is theoretically always busy with work.

**The Naive Idea vs. Reality**

Intuitively, you might think 100% utilization is the goal. "Every developer always working means we get the most done!" But queue theory, and real-world experience, tells us this isn't true in systems with **variability**.

Think back to the coffee shop or supermarket lines. Arrivals aren't perfectly spaced out, and service times aren't always the same (some tasks are quick, some take longer). This unpredictability is variability.

**The Problem with High Utilization (Approaching 100%)**

Queue theory models show that as utilization approaches 100%, even with small amounts of variability, queue lengths and waiting times don't just increase linearly – they explode *exponentially!*

Imagine traffic on a freeway. When it's at 60% capacity, traffic flows smoothly. At 80% capacity, it's heavier but usually still flowing. But get to 95% or 100% capacity, and one small incident (like someone braking suddenly, or a slightly slower driver) causes a massive, ripple-effect traffic jam. [Waiting times go through the roof](https://www.complexity-explorables.org/explorables/berlin-8-am/).

In a software project context:

- **Exploding Waiting Times (Cycle Time):** Tasks spend a disproportionately long time sitting in queues (in the backlog, waiting for review, waiting for deployment) as utilization gets very high. Your cycle time (*W* in Little's Law *L=λW*) goes up dramatically.
- **Increased WIP (L):** With longer waiting times and a steady arrival rate, the number of items in progress (L) balloons. High WIP leads to context switching, difficulty tracking status, increased risk, and delayed feedback.
- **Reduced Responsiveness:** The system becomes very brittle. A sudden influx of urgent bugs or a slightly underestimated task can completely overwhelm the system, as there's no capacity buffer to absorb the shock.
- **Stress and Burnout:** Constantly running at 100% capacity is unsustainable for a team. There's no time for refactoring, learning, collaboration, or dealing with the inevitable unplanned work.

Mathematically, the formulas for average waiting time in many queueing models have a term like 1/(1−ρ). As ρ (utilization) gets closer to 1, 1−ρ gets closer to 0, and 1/(1−ρ) shoots up towards infinity. This is the mathematical representation of the "queue explosion."

**The Problem with Low Utilization (e.g., 20-30%)**

While low utilization avoids the queue explosion problem, it's inefficient:

- **Wasted Capacity:** Your servers (team members) are idle a significant amount of the time, even if there's work waiting to be pulled. You're paying for capacity you're not using effectively.
- **Higher Cost:** The cost per completed work item is higher because your fixed capacity cost is spread over fewer completed items over time.

**Why \~80% is Often Ideal**

The \~80% utilization mark (it's not a magic number, but a common observation) represents a balance point:

1. **Good Efficiency:** Your servers (team) are busy a good portion of the time, ensuring work is being processed reasonably quickly.
2. **Manageable Queues & Waiting Times:** There's enough buffer capacity (the unused \~20%) to absorb typical fluctuations in arrivals and service times. Queues remain relatively short and predictable. Cycle times are much lower and more consistent than at higher utilization.
3. **Allows for slack:** The remaining capacity provides necessary "slack" for:
   - Handling unexpected high-priority work (bugs, production issues).
   - Investing in process improvement and reducing technical debt (which actually helps increase your service rate in the long run).
   - Learning and innovation.
   - Preventing burnout and maintaining a sustainable pace.

**In Summary:**

Queue theory teaches us that in systems with variability, pushing for 100% utilization is counterproductive. It leads to exponentially increasing queue lengths, ballooning cycle times, high WIP, and a brittle system. Aiming for a lower, sustainable utilization level like 80% provides a crucial buffer (slack) that allows the system to handle variability gracefully, resulting in much shorter and more predictable waiting times, lower WIP, and ultimately, higher *effective* throughput of valuable work delivered consistently over time. You might not be *theoretically* busy every single second, but the work flows through the system much faster and more reliably.

[Demo](https://aheckmann.github.io/queue-demos/)

## How to determine team capacity?

Determining a software development team's capacity for task assignment isn't an exact science because of the inherent variability (task complexity, interruptions, unforeseen issues, etc.). However, we can use principles derived from queue theory and practical agile/lean methodologies to get a realistic sense of what the team can sustainably deliver.

1. **Focus on Throughput (The Most Direct Measure):**

   - **What it is:** Throughput is the average number of completed and "done" work items (tasks, stories, bugs) over a specific period (e.g., per week, per sprint). This is your team's actual, historical service rate.
   - **Why it's best:** It's an empirical measure based on actual output flowing through your system, not just estimated effort. It automatically accounts for all the real-world overhead, interruptions, and variability that affect how much work truly gets finished and delivered.
   - **How to use it:**
     - Track the number of work items moved to a "Done" state each week or sprint cycle for several periods.
     - Calculate the average. This average throughput is your most reliable indicator of capacity for the next period, assuming similar conditions.
     - Look at the distribution of throughput – how much does it vary week to week? This variability tells you about the predictability (or lack thereof) of your system.

2. **Analyze Cycle Time:**

   - **What it is:** Cycle Time is the total time a work item spends in your system, from when work *starts* on it (or even when it enters the "ready" queue) until it's completed and delivered. It's the "Waiting Time in the System" (W) from Little's Law (L=λW). Analyzing W helps you understand the impact of your WIP (L) and Arrival Rate (λ) on flow.
   - **Why it's important:** While not a direct measure of *rate*, analyzing cycle time distribution gives you crucial insight into the health of your system and helps validate your capacity. Shorter, more consistent cycle times indicate work is flowing smoothly, often a sign that capacity is well-matched to demand and WIP is controlled. Long, highly variable cycle times suggest bottlenecks, excessive WIP, or unpredictable service times – all of which reduce effective capacity even if individual utilization seems high.
   - **How to use it:** Track the start and end time for each work item. Plot these on a chart (like a Scatterplot or [Cumulative Flow Diagram](https://en.wikipedia.org/wiki/Cumulative_flow_diagram). The typical range of cycle times tells you how long a task is likely to take to get through the system.

3. **Consider Average Work Item Size/Complexity:**

   - **What it is:** The estimated length of time needed to complete the given work. Teams often have an intuitive sense of how many items of a certain size range they can typically complete.
   - **How to use it:** It's more stable if your team consistently works on items within a similar size range. Understand that completing 5 small items might be faster and have less variability than completing 1 giant item. Focusing on smaller, similarly sized items helps make throughput more predictable.
   - **Stability:** Variability in service times (task complexity/size) is a key factor that causes queues to build up, especially at high utilization. Reducing variability in work item size can help stabilize the service rate.

4. **Account for Team Availability & Overhead:**

   - **What it is:** The number of people available and the time they spend on things *other* than working on primary project tasks.
   - **How to use it:** Before committing to a certain capacity for an upcoming period, manually adjust based on planned absences (vacations, holidays, training) or known significant unavoidable meetings. A simple rule of thumb might be to reduce expected capacity proportionally to the reduction in available person-days.
   - **Why:** Absences and overhead reduce the total available "server hours," effectively lowering the potential maximum service rate for that period.

5. **Identify and Address Bottlenecks:**

   - **What it is:** Any stage in your workflow where work tends to pile up or slow down (e.g., code review, testing, deployment).
   - **Why it's important:** The capacity of the *entire system* is limited by the capacity of its biggest bottleneck. You can't increase overall throughput by increasing capacity *before* or *after* the bottleneck; you have to fix the bottleneck itself.
   - **How to use it:** Use visualization tools (like Kanban boards) and flow metrics (like Cycle Time breakdown by stage) to spot where work is spending the most time waiting or being processed slowly. Address the constraints at these points to truly increase system capacity.
   - **Queue Theory Connection:** Bottlenecks are stages with service rates lower than the arrival rate *into that stage*, causing queues to form and grow.

**Putting it Together:**

The most robust way to determine and manage capacity involves combining these approaches:

1. **Establish a baseline capacity using historical Throughput.** This is your most reliable guide to what your team can actually finish.
2. **Monitor Cycle Time** to understand the flow and identify where delays are occurring, indicating potential capacity issues or process problems.
3. **Rely on the count of completed items** (throughput) as your primary capacity metric. Only use work item sizing (like points) as a way to understand the relative size of upcoming work as compared to the size of work completed historically.
4. **Adjust your forecast based on known availability changes.**
5. **Continuously identify and work to eliminate bottlenecks** to increase your team's sustainable service rate (throughput) over time.

By focusing on the flow of work items and your historical completion rate (throughput), you get a much more realistic picture of your team's capacity than by just estimating effort or trying to keep everyone 100% busy. This aligns directly with the principles of queue theory, helping you manage variability and improve the predictability and efficiency of your software development process.
