# OS316
# Adaptive OS Scheduler for Real-Time

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_TASKS 10
typedef struct {
    int id;
    char name[20];
    int priority;
    int deadline;
    int execution_time;
} Task;
Task task_queue[MAX_TASKS];
int task_count = 0;
void add_task() {
    if (task_count >= MAX_TASKS) {
        printf("Task limit reached!\n");
        return;
    }
    Task t;
    t.id = task_count + 1;
    printf("Enter task name: ");
    scanf("%s", t.name);
    printf("Enter priority (1=High, 10=Low): ");
    scanf("%d", &t.priority);
    printf("Enter deadline (in ms): ");
    scanf("%d", &t.deadline);
    printf("Enter execution time (in ms): ");
    scanf("%d", &t.execution_time);
    task_queue[task_count++] = t;
    printf("Task added successfully.\n");
}
void display_tasks() {
    if (task_count == 0) {
        printf("No tasks scheduled.\n");
        return;
    }
    printf("\nScheduled Tasks:\n");
    printf("ID\tName\tPriority\tDeadline\tExec Time\n");
    for (int i = 0; i < task_count; i++) {
        Task t = task_queue[i];
        printf("%d\t%s\t%d\t\t%d\t\t%d\n", t.id, t.name, t.priority, t.deadline, t.execution_time);
    }
}
void adaptive_schedule() {
    if (task_count == 0) {
        printf("No tasks to schedule.\n");
        return;
    }
    for (int i = 0; i < task_count - 1; i++) {
        for (int j = i + 1; j < task_count; j++) {
            if (task_queue[i].priority > task_queue[j].priority || 
                (task_queue[i].priority == task_queue[j].priority && task_queue[i].deadline > task_queue[j].deadline)) {
                Task temp = task_queue[i];
                task_queue[i] = task_queue[j];
                task_queue[j] = temp;
            }
        }
    }
    printf("\nAdaptive Scheduler Execution Order:\n");
    for (int i = 0; i < task_count; i++) {
        Task t = task_queue[i];
        printf("Executing Task %s (Priority: %d, Deadline: %d, Execution Time: %dms)\n",
               t.name, t.priority, t.deadline, t.execution_time);
    }
}
void interface() {
    int choice;
    while (1) {
        printf("\n=== Adaptive Real-Time Scheduler ===\n");
        printf("1. Add Task\n");
        printf("2. View Scheduled Tasks\n");
        printf("3. Run Adaptive Scheduler\n");
        printf("4. Exit\n");
        printf("Select an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: add_task(); break;
            case 2: display_tasks(); break;
            case 3: adaptive_schedule(); break;
            case 4: exit(0);
            default: printf("Invalid choice. Try again.\n");
        }
    }
}

int main() {
    interface();
    return 0;
}
