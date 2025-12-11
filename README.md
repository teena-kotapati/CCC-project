#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_STUDENTS 100


typedef struct {
    char username[20];
    char password[20];
    char role[10];
} Credential;

Credential creds[] = {
    {"admin", "admin123", "admin"},
    {"staff", "staffpass", "staff"},
    {"guest", "guestpass", "guest"}
};
int totalCreds = 3;

// Student structure
typedef struct {
    int roll;
    char name[50];
    float mark;
} Student;

Student students[MAX_STUDENTS];
int studentCount = 3; // initial sample students

char currentUser[50];
char currentRole[20];

// Sample initial students
void initStudents() {
    students[0].roll = 1; strcpy(students[0].name, "Alice"); students[0].mark = 85.5;
    students[1].roll = 2; strcpy(students[1].name, "Bob");   students[1].mark = 78.0;
    students[2].roll = 3; strcpy(students[2].name, "Charlie"); students[2].mark = 92.0;
}

// Login function
int login() {
    char inUser[50], inPass[50];
    printf("USERNAME: ");
    scanf("%s", inUser);
    printf("PASSWORD: ");
    scanf("%s", inPass);

    for(int i=0; i<totalCreds; i++) {
        if(strcmp(inUser, creds[i].username)==0 && strcmp(inPass, creds[i].password)==0) {
            strcpy(currentUser, inUser);
            strcpy(currentRole, creds[i].role);
            return 1;
        }
    }
    return 0;
}

// Student operations
void addStudent() {
    if(studentCount >= MAX_STUDENTS) {
        printf("Student limit reached!\n");
        return;
    }

    Student s;
    printf("Roll: "); scanf("%d", &s.roll);
    printf("Name: "); scanf(" %[^\n]", s.name);
    printf("Mark: "); scanf("%f", &s.mark);

    students[studentCount++] = s;
    printf("Student added!\n");
}

void displayStudents() {
    printf("Roll\tName\tMark\n");
    printf("----\t----\t----\n");
    for(int i=0; i<studentCount; i++)
        printf("%d\t%s\t%.2f\n", students[i].roll, students[i].name, students[i].mark);
}

void searchStudent() {
    int find;
    printf("Enter roll to search: ");
    scanf("%d", &find);
    for(int i=0;i<studentCount;i++) {
        if(students[i].roll == find) {
            printf("Found: %d %s %.2f\n", students[i].roll, students[i].name, students[i].mark);
            return;
        }
    }
    printf("Student not found!\n");
}

void updateStudent() {
    int updateRoll;
    printf("Enter roll to update: ");
    scanf("%d", &updateRoll);
    for(int i=0;i<studentCount;i++) {
        if(students[i].roll == updateRoll) {
            printf("New Name: "); scanf(" %[^\n]", students[i].name);
            printf("New Mark: "); scanf("%f", &students[i].mark);
            printf("Student updated!\n");
            return;
        }
    }
    printf("Roll not found!\n");
}

void deleteStudent() {
    int delRoll;
    printf("Enter roll to delete: ");
    scanf("%d", &delRoll);
    for(int i=0;i<studentCount;i++) {
        if(students[i].roll == delRoll) {
            for(int j=i;j<studentCount-1;j++)
                students[j] = students[j+1];
            studentCount--;
            printf("Student deleted!\n");
            return;
        }
    }
    printf("Roll not found!\n");
}

// Menus
void adminMenu() {
    int c;
    while(1) {
        printf("\nADMIN MENU\n1.Add\n2.Display\n3.Search\n4.Update\n5.Delete\n6.Logout\n");
        scanf("%d",&c);
        if(c==1)addStudent();
        else if(c==2)displayStudents();
        else if(c==3)searchStudent();
        else if(c==4)updateStudent();
        else if(c==5)deleteStudent();
        else return;
    }
}

void staffMenu() {
    int c;
    while(1) {
        printf("\nSTAFF MENU\n1.Add\n2.Display\n3.Search\n4.Update\n5.Logout\n");
        scanf("%d",&c);
        if(c==1)addStudent();
        else if(c==2)displayStudents();
        else if(c==3)searchStudent();
        else if(c==4)updateStudent();
        else return;
    }
}

void guestMenu() {
    int c;
    while(1) {
        printf("\nGUEST MENU\n1.Display\n2.Search\n3.Logout\n");
        scanf("%d",&c);
        if(c==1)displayStudents();
        else if(c==2)searchStudent();
        else return;
    }
}

int main() {
    initStudents();
    if(!login()) {
        printf("Invalid login!\n");
        return 0;
    }
    printf("Logged in as: %s\n", currentRole);

    if(strcmp(currentRole,"admin")==0) adminMenu();
    else if(strcmp(currentRole,"staff")==0) staffMenu();
    else guestMenu();

    return 0;
}
