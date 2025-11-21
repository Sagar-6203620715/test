/* fcfs.c
   First-Come First-Serve scheduling (non-preemptive)
   Input: number of processes, then for each: name arrival_time burst_time
   Output: start, finish, turnaround, waiting times and averages
*/

#include <stdio.h>
#include <string.h>

int main() {
    int n;
    printf("Enter number of processes: ");
    if (scanf("%d", &n) != 1 || n <= 0) {
        printf("Invalid number of processes.\n");
        return 1;
    }

    char pname[50][20];
    int at[50], bt[50];
    int i, j;

    for (i = 0; i < n; ++i) {
        printf("Enter ProcessName ArrivalTime BurstTime (e.g. P1 0 5): ");
        scanf("%19s %d %d", pname[i], &at[i], &bt[i]);
    }

    // Sort by arrival time (stable bubble-sort to preserve input order for ties)
    for (i = 0; i < n-1; ++i) {
        for (j = 0; j < n-1-i; ++j) {
            if (at[j] > at[j+1]) {
                // swap arrival
                int tmpi = at[j]; at[j] = at[j+1]; at[j+1] = tmpi;
                // swap burst
                tmpi = bt[j]; bt[j] = bt[j+1]; bt[j+1] = tmpi;
                // swap name
                char tmpname[20];
                strcpy(tmpname, pname[j]);
                strcpy(pname[j], pname[j+1]);
                strcpy(pname[j+1], tmpname);
            }
        }
    }

    int start[50], finish[50], tat[50], wt[50];
    int cur_time = 0;
    long total_wt = 0, total_tat = 0;

    for (i = 0; i < n; ++i) {
        if (cur_time < at[i]) cur_time = at[i];  // CPU idle until process arrives
        start[i] = cur_time;
        finish[i] = start[i] + bt[i];
        tat[i] = finish[i] - at[i];
        wt[i] = tat[i] - bt[i];

        total_wt += wt[i];
        total_tat += tat[i];

        cur_time = finish[i];
    }

    printf("\nPName\tArr\tBurst\tStart\tFinish\tTAT\tWT\n");
    for (i = 0; i < n; ++i) {
        printf("%s\t%d\t%d\t%d\t%d\t%d\t%d\n",
               pname[i], at[i], bt[i], start[i], finish[i], tat[i], wt[i]);
    }

    printf("\nAverage Waiting Time = %.2f\n", (double)total_wt / n);
    printf("Average Turnaround Time = %.2f\n", (double)total_tat / n);

    return 0;
}


#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main()
{
    int i, pid;
    pid = fork();

    if (pid == -1)
    {
        printf("fork failed");
        exit(0);
    }
    else if (pid == 0)
    {
        printf("\nChild process starts");
        for (i = 0; i < 5; i++)
        {
            printf("\nChild process %d is called", i);
        }
        printf("\nChild process ends");
    }
    else
    {
        wait(0);
        printf("\nParent process ends");
    }

    return 0;
}
