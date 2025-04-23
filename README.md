# OS316
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h> 

#define MAX_TASKS 5

typedef struct {
    int id;
    int priority;            
    int exec_time_ms;        
    int deadline_ms;        
    int deadline_miss_count;
} Task;

void run_task(Task* task) {
    printf("Running Task %d [Priority: %d, Exec Time: %dms, Deadline: %dms]\n",
           task->id, task->priority, task->exec_time_ms, task->deadline_ms);
    usleep(task->exec_time_ms * 1000);  

    int rand_val = rand() % 10;
    if (rand_val < 3) {  // 30% chance
        task->deadline_miss_count++;
        printf("⚠️  Task %d missed its deadline!\n", task->id);
    }
}

void adaptive_scheduler(Task tasks[], int n) {
    for (int i = 0; i < n; i++) {
        if (tasks[i].deadline_miss_count >= 2 && tasks[i].priority > 1) {
            tasks[i].priority--;  
            tasks[i].deadline_miss_count = 0;
            printf("🔁 Task %d priority increased to %d (due to deadline misses)\n",
                   tasks[i].id, tasks[i].priority);
        }
    }
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (tasks[i].priority > tasks[j].priority) {
                Task temp = tasks[i];
                tasks[i] = tasks[j];
                tasks[j] = temp;
            }
        }
    }
    for (int i = 0; i < n; i++) {
        run_task(&tasks[i]);
    }
}

int main() {
    srand(0);

    Task tasks[MAX_TASKS] = {
        {1, 3, 100, 150, 0},
        {2, 2, 80, 120, 0},
        {3, 1, 50, 70, 0},
        {4, 4, 120, 200, 0},
        {5, 2, 90, 100, 0}
    };

    for (int cycle = 1; cycle <= 5; cycle++) {
        printf("\n=== Scheduling Cycle %d ===\n", cycle);
        adaptive_scheduler(tasks, MAX_TASKS);
    }

    return 0;
}
