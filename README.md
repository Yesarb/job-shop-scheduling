# Job-shop scheduling for a watchmaker

Project for the course Production Planning and Scheduling (HEC Lausanne). The case is a fictional luxury watchmaker, Aurelius SA, which makes two models, Heritage and Chronos.

The problem is to schedule 12 watches from 5 customer orders through a workshop with 5 stations. The two models follow different routings, the assembly station has two parallel machines, and each order has a release date, a due date and a priority. I formulated it as a mixed-integer linear program with big-M disjunctive constraints, written with PuLP and solved with CBC. The objective is the makespan plus the weighted tardiness, with weights 3, 2 and 1 for priority 1, 2 and 3 orders. The full analysis is in notebooks/job_shop_scheduling.ipynb, with the outputs and Gantt charts saved so GitHub displays them directly.

After the base schedule, I tested four changes: a zero-wait policy for Chronos, cleaning times when a station switches between models, a 16-hour breakdown of the crystal fitting station, and a last-minute VIP order. The notebook also has sensitivity tables for the number of assembly machines, tighter due dates, the cleaning time and the length of the breakdown.

## Results

The base schedule finishes all 12 watches in 45.5 hours with no late order, and no station is heavily loaded (inspection is the busiest at 61.5%). A zero-wait policy for Chronos and cleaning times of up to 7 hours per model switch change neither the makespan nor the tardiness, so the workshop has enough slack to absorb them. The second assembly machine shortens the schedule: with a single machine, the best solution found has a makespan of 49.0 hours (3.5 hours longer) and no late job, while a third machine brings nothing. The breakdown is different: with the crystal fitting station out for 16 hours from hour 30, the makespan goes to 54.5 hours and orders 2 and 5 are late by 9.5 and 4.5 hours. The last-minute VIP order can be fitted in and finished at hour 23.5, exactly on its due date, without delaying any other order.

## Limitations

The instance is small and deterministic: processing times, release dates and due dates are given, and nothing is random. Each solve has a time limit of 120 seconds (200 for the breakdown model), and a few runs in the sensitivity tables stopped at it, so those rows show the best solution found and not a proven optimum. The notebook lists which ones. The base schedule, the zero-wait policy and the 16-hour breakdown were solved to optimality.

## Running the notebook

The notebook defines all its data, so there is nothing to download. PuLP installs the CBC solver with it.

    pip install -r requirements.txt
    jupyter lab notebooks/job_shop_scheduling.ipynb

A full run takes about half an hour on my laptop, mostly because of the sensitivity analyses.
