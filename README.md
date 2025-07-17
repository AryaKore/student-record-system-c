#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    int id;
    char name[50];
    float marks;
};

void addStudent(FILE *fp) {
    struct Student s;
    printf("Enter ID: ");
    scanf("%d", &s.id);
    printf("Enter Name: ");
    scanf(" %[^\n]", s.name);
    printf("Enter Marks: ");
    scanf("%f", &s.marks);

    fwrite(&s, sizeof(s), 1, fp);
    printf("Student added!\n");
}

void displayStudents(FILE *fp) {
    struct Student s;
    rewind(fp);
    printf("\n--- Student Records ---\n");
    while (fread(&s, sizeof(s), 1, fp)) {
        printf("ID: %d | Name: %s | Marks: %.2f\n", s.id, s.name, s.marks);
    }
}

void searchStudent(FILE *fp) {
    struct Student s;
    int id, found = 0;
    printf("Enter ID to search: ");
    scanf("%d", &id);
    rewind(fp);

    while (fread(&s, sizeof(s), 1, fp)) {
        if (s.id == id) {
            printf("Found -> ID: %d | Name: %s | Marks: %.2f\n", s.id, s.name, s.marks);
            found = 1;
            break;
        }
    }
    if (!found) printf("Student not found.\n");
}

int main() {
    FILE *fp = fopen("students.dat", "ab+");
    if (!fp) {
        perror("File open error");
        return 1;
    }

    int choice;
    do {
        printf("\n1. Add Student\n2. Display All\n3. Search\n4. Exit\nChoose: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: addStudent(fp); break;
            case 2: displayStudents(fp); break;
            case 3: searchStudent(fp); break;
            case 4: break;
            default: printf("Invalid option.\n");
        }
    } while (choice != 4);

    fclose(fp);
    return 0;
}

