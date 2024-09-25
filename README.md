# OmkumarMangukiya.github.io
## Hello my name is Omkumar Mangukiya
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define MAX_SIZE 100

int heap[MAX_SIZE];
int heapSize = 0;

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void heapifyUp(int index) {
    while (index > 0) {
        int parent = (index - 1) / 2;
        if (heap[index] < heap[parent]) {
            swap(&heap[index], &heap[parent]);
            index = parent;
        } else {
            break;
        }
    }
}

void heapifyDown(int index) {
    int smallest = index;
    int left = 2 * index + 1;
    int right = 2 * index + 2;

    if (left < heapSize && heap[left] < heap[smallest]) {
        smallest = left;
    }
    if (right < heapSize && heap[right] < heap[smallest]) {
        smallest = right;
    }
    if (smallest != index) {
        swap(&heap[index], &heap[smallest]);
        heapifyDown(smallest);
    }
}

void insert(int key) {
    if (heapSize == MAX_SIZE) {
        printf("Heap is full!\n");
        return;
    }
    heap[heapSize] = key;
    heapSize++;
    heapifyUp(heapSize - 1);
}

int pop() {
    if (heapSize == 0) {
        printf("Heap is empty!\n");
        return -1; // Return an invalid value
    }
    int root = heap[0];
    heap[0] = heap[heapSize - 1]; // Move the last element to the root
    heapSize--; // Decrease the heap size
    heapifyDown(0); // Restore heap property
    return root; // Return the root value
}

void buildHeap(int elements[], int count) {
    for (int i = 0; i < count; i++) {
        heap[i] = elements[i];
    }
    heapSize = count;
    for (int i = (heapSize / 2) - 1; i >= 0; i--) {
        heapifyDown(i);
    }
}

void heapSort(int sortedArray[]) {
    int originalSize = heapSize;
    for (int i = 0; i < originalSize; i++) {
        sortedArray[i] = pop(); // Pop elements into sorted array
    }
    // Restore heapSize after sorting
    heapSize = originalSize; 
    for (int i = 0; i < originalSize; i++) {
        heap[i] = sortedArray[i]; // Restore the original heap values
    }
}

void displayHeap() {
    printf("Current heap: ");
    for (int i = 0; i < heapSize; i++) {
        printf("%d ", heap[i]);
    }
    printf("\n");
}

int compare(const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}

int main() {
    srand(time(NULL));
    int choice, element, n;

    while (1) {
        printf("\n1. Create an empty min heap\n");
        printf("2. Input a comma-separated list of positive integers\n");
        printf("3. Insert an element into the heap\n");
        printf("4. Pop an element from the heap\n");
        printf("5. Display sorted integers from the heap\n");
        printf("6. Generate a random array and sort it\n");
        printf("7. Quit\n");
        printf("Choose an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                heapSize = 0;
                printf("Created an empty min heap.\n");
                break;
            case 2: {
                char input[256];
                printf("Enter comma-separated positive integers: ");
                scanf(" %[^\n]", input); // Read the entire line of input
                char *token = strtok(input, ",");
                int count = 0;
                int elements[MAX_SIZE];

                while (token != NULL) {
                    if (count >= MAX_SIZE) {
                        printf("Too many elements entered. Maximum allowed is %d.\n", MAX_SIZE);
                        break; // Prevent overflow
                    }
                    elements[count++] = atoi(token);
                    token = strtok(NULL, ",");
                }
                buildHeap(elements, count);
                printf("Heap created with %d elements.\n", count);
                break;
            }
            case 3:
                printf("Enter a positive integer to insert: ");
                scanf("%d", &element);
                insert(element);
                printf("Inserted %d into the heap.\n", element);
                break;
            case 4: {
                int poppedElement = pop();
                if (poppedElement != -1) {
                    printf("Popped element: %d\n", poppedElement);
                }
                break;
            }
            case 5: {
                int sortedArray[MAX_SIZE];
                printf("Heap before sorting:\n");
                displayHeap(); // Display current heap before sorting
                heapSort(sortedArray);
                printf("Sorted elements from heap: ");
                for (int i = 0; i < heapSize; i++) {
                    printf("%d ", sortedArray[i]);
                }
                printf("\n");
                break;
            }
            case 6: {
                printf("Enter the length of the random array: ");
                scanf("%d", &n);
                if (n > MAX_SIZE) {
                    printf("Maximum allowed is %d.\n", MAX_SIZE);
                    n = MAX_SIZE;
                }
                int randomArray[MAX_SIZE];
                for (int i = 0; i < n; i++) {
                    randomArray[i] = rand() % 90000 + 10000; // [10000, 100000)
                }
                printf("Generated array: ");
                for (int i = 0; i < n; i++) {
                    printf("%d ", randomArray[i]);
                }
                printf("\n");

                clock_t start = clock();
                buildHeap(randomArray, n);
                int sortedWithHeap[MAX_SIZE];
                heapSort(sortedWithHeap);
                clock_t end = clock();
                double heapSortTime = (double)(end - start) / CLOCKS_PER_SEC;

                start = clock();
                qsort(randomArray, n, sizeof(int), compare);
                end = clock();
                double librarySortTime = (double)(end - start) / CLOCKS_PER_SEC;

                printf("Sorted with heap sort: ");
                for (int i = 0; i < n; i++) {
                    printf("%d ", sortedWithHeap[i]);
                }
                printf("\nHeap sort time: %.6f seconds\n", heapSortTime);
                printf("Library sort time: %.6f seconds\n", librarySortTime);
                break;
            }
            case 7:
                printf("Quitting the program.\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}
## https://github.com/OmkumarMangukiya/MERNstack-cohort
