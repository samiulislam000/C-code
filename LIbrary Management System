#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Admin
{
    char username[20];
    char password[20];
};

struct Book
{
    int id;
    char title[50];
    char author[50];
    int quantity;
};

struct Student
{
    int studentId;
    char department[30];
    char semester[10];
    char section[5];
    char mobile[15];
    char gmail[50];
    char password[20];
};

struct Issue
{
    int studentId;
    int bookId;
    char issueDate[12];
    char returnDate[12];
};

// important functions

// Clear IntupT Buffer
void clear_input_buffer()
{
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

// IS any previos Student id or Same student Id checker
int isStudentIdExists(int studentId)
{
    FILE *fp = fopen("students.dat", "rb");
    if (fp == NULL) return 0;
    struct Student s;
    int exists = 0;
    while(fread(&s, sizeof(s), 1, fp))
        {
        if(s.studentId == studentId)
        {
            exists = 1;
            break;
        }
    }
    fclose(fp);
    return exists;
}

// FUNCTION DECLARATIONS
void createAdminAccount();
void createStudentAccount();
int adminLogin();
int studentLogin();
void adminMenu();
void studentMenu(int studentId);
void addBook();
void viewBooks();
void deleteBook();
void studentList();
void searchBook();
void issueBook(int studentId);
void returnBook();

// Hear,s Start MAIN FUNCTION
int main()
{
    int choice;
    while(1) {
        printf("\n===== LIBRARY MANAGEMENT SYSTEM =====\n");
        printf("1. Create Admin Account\n2. Create Student Account\n3. Admin Login\n4. Student Login\n5. Exit\n");
        printf("Enter your choice: ");
        if (scanf("%d", &choice) != 1)
            {
            printf("Invalid input. Please enter a number.\n");
            clear_input_buffer();
            continue;
        }
        clear_input_buffer();

        switch(choice)
        {
            case 1: createAdminAccount(); break;
            case 2: createStudentAccount(); break;
            case 3: if(adminLogin()) adminMenu(); break;
            case 4: {
                int sid = studentLogin();
                if(sid != 0) studentMenu(sid);
                break;
            }
            case 5: exit(0);
            default: printf("Invalid choice!\n");
        }
    }
    return 0;
}

// CREATE ADMIN ACCOUNT Section
void createAdminAccount()
{
    struct Admin a;
    FILE *fpAdmin = fopen("admin.dat","wb");
    if(fpAdmin == NULL) { printf("Error opening file for admin!\n"); return;
    }

    printf("Enter Admin Username: "); fgets(a.username, sizeof(a.username), stdin);
    a.username[strcspn(a.username,"\n")] = 0;

    printf("Enter Admin Password: "); fgets(a.password, sizeof(a.password), stdin);
    a.password[strcspn(a.password,"\n")] = 0;

    fwrite(&a,sizeof(a),1,fpAdmin);
    fclose(fpAdmin);
    printf("Admin account created successfully!\n");
}

// CREATE STUDENT ACCOUNT SECTION
void createStudentAccount()
{
    struct Student s;
    int tempId;

    printf("Enter Student ID: ");
    if (scanf("%d",&tempId) != 1)
        {
        printf("Invalid input for ID.\n");
        clear_input_buffer();
        return;
    }
    clear_input_buffer();

    if (isStudentIdExists(tempId))
        {
        printf("Error: Student ID %d already exists. Account creation failed.\n", tempId);
        return;
    }
    s.studentId = tempId;

    FILE *fpStudent = fopen("students.dat","ab");
    if(fpStudent == NULL) { printf("File error!\n"); return;
    }

    printf("Enter Department: "); fgets(s.department, sizeof(s.department), stdin);
    s.department[strcspn(s.department,"\n")] = 0;

    printf("Enter Semester: "); fgets(s.semester, sizeof(s.semester), stdin);
    s.semester[strcspn(s.semester,"\n")] = 0;

    printf("Enter Section: "); fgets(s.section, sizeof(s.section), stdin);
    s.section[strcspn(s.section,"\n")] = 0;

    printf("Enter Mobile Number: "); fgets(s.mobile, sizeof(s.mobile), stdin);
    s.mobile[strcspn(s.mobile,"\n")] = 0;

    printf("Enter Gmail ID: "); fgets(s.gmail, sizeof(s.gmail), stdin);
    s.gmail[strcspn(s.gmail,"\n")] = 0;

    printf("Enter Password: "); fgets(s.password, sizeof(s.password), stdin);
    s.password[strcspn(s.password,"\n")] = 0;

    fwrite(&s,sizeof(s),1,fpStudent);
    fclose(fpStudent);
    printf("Student account created successfully!\n");
}

// ADMIN LOGIN SECTION
int adminLogin()
{
    struct Admin a;
    char user[20], pass[20];
    FILE *fpAdmin = fopen("admin.dat","rb");
    if(fpAdmin == NULL) { printf("No admin account found! Please create one (Option 1).\n"); return 0;
    }

    printf("Enter Admin Username: "); fgets(user, sizeof(user), stdin);
    user[strcspn(user,"\n")] = 0;

    printf("Enter Admin Password: "); fgets(pass, sizeof(pass), stdin);
    pass[strcspn(pass,"\n")] = 0;

    if(fread(&a,sizeof(a),1,fpAdmin) != 1)
        {
        printf("Admin file is empty or corrupt.\n");
        fclose(fpAdmin);
        return 0;
    }
    fclose(fpAdmin);

    if(strcmp(user,a.username)==0 && strcmp(pass,a.password)==0)
        {
        printf("Login Successful!\n");
        return 1;
    } else {
        printf("Invalid credentials!\n");
        return 0;
    }
}

// STUDENT LOGIN SECTION
int studentLogin()
{
    int sid;
    char pass[20];
    struct Student s;
    FILE *fpStudent = fopen("students.dat","rb");

    if(fpStudent == NULL)
        {
        printf("No student accounts found!\n");
        return 0;
    }

    printf("Enter Student ID: ");
    if (scanf("%d",&sid) != 1)
    {

        printf("Invalid input for ID.\n");
        clear_input_buffer();
        return 0;
    }
    clear_input_buffer();

    printf("Enter Password: "); fgets(pass,sizeof(pass),stdin);
    pass[strcspn(pass,"\n")] = 0;

    while(fread(&s,sizeof(s),1,fpStudent))
        {
        if(s.studentId==sid && strcmp(s.password,pass)==0)
        {

            fclose(fpStudent);
            printf("Login Successful!\n");
            return sid;
        }
    }
    fclose(fpStudent);
    printf("Invalid ID or Password!\n");
    return 0;
}

// ADMIN MENU SECTION
void adminMenu()
{
    int choice;
    while(1)
        {
        printf("\n--- ADMIN MENU ---\n");
        printf("1. View Book List\n2. Add New Book\n3. View Student Accounts & Issued Books\n4. Delete Book\n5. Register Book Return\n6. Logout\n");
        printf("Enter choice: ");
        if (scanf("%d",&choice) != 1)
        {
            printf("Invalid input. Please enter a number.\n");
            clear_input_buffer();
            continue;
        }
        clear_input_buffer();

        switch(choice)
        {
            case 1: viewBooks(); break;
            case 2: addBook(); break;
            case 3: studentList(); break;
            case 4: deleteBook(); break;
            case 5: returnBook(); break;
            case 6: return;
            default: printf("Invalid choice!\n");
        }
    }
}

// STUDENT MENU SECTION
void studentMenu(int studentId)
{
    int choice;
    while(1)
        {
        printf("\n--- STUDENT MENU ---\n");
        printf("1. View All Books\n2. Search Book by Name\n3. Issue Book\n4. Logout\n");
        printf("Enter choice: ");
        if (scanf("%d",&choice) != 1) {
            printf("Invalid input. Please enter a number.\n");
            clear_input_buffer();
            continue;
        }
        clear_input_buffer();

        switch(choice)
        {
            case 1: viewBooks(); break;
            case 2: searchBook(); break;
            case 3: issueBook(studentId); break;
            case 4: return;
            default: printf("Invalid choice!\n");
        }
    }
}

//  ADD BOOK SECTION
void addBook()
{
    struct Book b;
    FILE *fpBook = fopen("books.dat","ab");
    if(fpBook==NULL)
        {
            printf("Error opening file!\n"); return;
        }

    printf("Enter Book ID: ");
    if (scanf("%d",&b.id) != 1)
        {
        printf("Invalid input for ID. Book not added.\n");
        clear_input_buffer();
        fclose(fpBook);
        return;
        }
    clear_input_buffer();

    printf("Enter Book Title: "); fgets(b.title,sizeof(b.title),stdin);
    b.title[strcspn(b.title,"\n")] = 0;

    printf("Enter Author: "); fgets(b.author,sizeof(b.author),stdin);
    b.author[strcspn(b.author,"\n")] = 0;

    printf("Enter Quantity: ");
    if (scanf("%d",&b.quantity) != 1)
        {
        printf("Invalid input for Quantity. Book not added.\n");
        clear_input_buffer();
        fclose(fpBook);
        return;
        }
    clear_input_buffer();

    fwrite(&b,sizeof(b),1,fpBook);
    fclose(fpBook);
    printf("Book added successfully!\n");
}

// VIEW BOOKS SECTION
void viewBooks()
{
    struct Book b;
    FILE *fpBook = fopen("books.dat","rb");
    if(fpBook==NULL)
        {
        printf("No books found!\n");
        return;
        }

    printf("\n--- BOOK LIST ---\n");
    printf("| %-5s| %-55s| %-25s| %-10s|\n", "ID", "TITLE", "AUTHOR", "QUANTITY");
    printf("|------|---------------------------------------------------------|--------------------------|-----------|\n");

    int book_count = 0;
    while(fread(&b,sizeof(b),1,fpBook)) {
        printf("| %-5d| %-55s| %-25s| %-10d|\n", b.id, b.title, b.author, b.quantity);
        book_count++;
    }
    printf("|------|---------------------------------------------------------|--------------------------|-----------|\n");

    if (book_count == 0) printf("The library has no books currently.\n");

    fclose(fpBook);
}

// DELETE BOOK SECTION
void deleteBook()
{
    int id, found=0;
    struct Book b;
    FILE *fpBook = fopen("books.dat","rb");
    if(fpBook==NULL)
        {
            printf("No books found to delete!\n"); return;
        }

    FILE *ft = fopen("temp.dat","wb");
    if (ft == NULL)
        {
        printf("Error creating temporary file.\n");
        fclose(fpBook);
        return;
        }

    printf("Enter Book ID to delete: ");
    if (scanf("%d",&id) != 1)
        {
        printf("Invalid input for ID.\n");
        clear_input_buffer();
        fclose(fpBook);
        fclose(ft);
        return;
        }
    clear_input_buffer();

    while(fread(&b,sizeof(b),1,fpBook))
        {
        if(b.id != id) {
            fwrite(&b,sizeof(b),1,ft);
        }
        else
        {
            found=1;
        }
    }
    fclose(fpBook);
    fclose(ft);

    remove("books.dat");
    rename("temp.dat","books.dat");

    if(found)
        {
        printf("Book ID %d deleted successfully!\n", id);
        }
    else
        {
        printf("Book ID %d not found!\n", id);
        }
}

// STUDENT LIST + BOOK ISSUES SECTION
void studentList()
{
    struct Student s;
    struct Issue i;
    struct Book b;
    FILE *fpStudent = fopen("students.dat","rb");

    if(fpStudent==NULL)
        {
        printf("No student accounts found!\n");
        return;
        }

    printf("\n--- STUDENT ACCOUNT LIST ---\n");
    printf("| %-12s| %-25s| %-10s| %-5s| %-15s| %-40s|\n", "ID", "DEPARTMENT", "SEMESTER", "SEC", "MOBILE", "GMAIL");
    printf("|------------|-------------------------|------------|-----|-----------------|------------------------------------------|\n");

    while(fread(&s,sizeof(s),1,fpStudent)) {
        printf("| %-12d| %-25s| %-10s| %-5s| %-15s| %-40s|\n", s.studentId, s.department, s.semester, s.section, s.mobile, s.gmail);

        FILE *fpIssue = fopen("issues.dat","rb");
        if(fpIssue != NULL)
            {
            int has_issues = 0;
            while(fread(&i,sizeof(i),1,fpIssue))
            {
                if(i.studentId == s.studentId)
                {
                    has_issues = 1;

                    FILE *fpBook = fopen("books.dat","rb");
                    char bookTitle[50] = "Unknown Book";

                    if (fpBook != NULL)
                        {
                        while(fread(&b,sizeof(b),1,fpBook))
                        {
                            if(b.id == i.bookId)
                            {
                                strncpy(bookTitle, b.title, 49);
                                bookTitle[49] = '\0';
                                break;
                            }
                        }
                        fclose(fpBook);
                    }
                    printf(" \t\t--> Issued Book: %-30s | Issue Date: %-10s | Return Date: %-10s\n",
                           bookTitle, i.issueDate, i.returnDate);
                }
            }
            if (!has_issues)
                {
                printf(" \t\t--> No books currently issued.\n");
                }
            fclose(fpIssue);
        }
        else
            {
             printf(" \t\t--> No issues record file found.\n");
            }
        printf("|------------|-------------------------|------------|-----|-----------------|------------------------------------------|\n");
    }
    fclose(fpStudent);
}

// SEARCH BOOK SECTION
void searchBook()
{
    char name[50];
    int found=0;
    struct Book b;

    printf("Enter book name (or part of the name) to search: ");
    fgets(name,sizeof(name),stdin);
    name[strcspn(name,"\n")] = 0;

    FILE *fpBook = fopen("books.dat","rb");
    if(fpBook==NULL)
        {
            printf("No books found!\n"); return;
        }

    printf("\n--- SEARCH RESULTS ---\n");
    printf("| %-5s| %-55s| %-25s| %-10s|\n", "ID", "TITLE", "AUTHOR", "QUANTITY");
    printf("|------|---------------------------------------------------------|--------------------------|-----------|\n");

    while(fread(&b,sizeof(b),1,fpBook))
        {
        if(strstr(b.title,name)!=NULL)
        {
            printf("| %-5d| %-55s| %-25s| %-10d|\n", b.id, b.title, b.author, b.quantity);
            found=1;
        }
    }

    printf("|------|---------------------------------------------------------|--------------------------|-----------|\n");
    if(!found) printf("Book '%s' not found!\n", name);

    fclose(fpBook);
}

// ISSUE BOOK SECTION (Quantity Update Included)
void issueBook(int studentId)
{
    struct Issue i;
    struct Book b, tempB;
    int bookId, found=0;
    long book_pos = -1;

    printf("Enter Book ID to issue: ");
    if (scanf("%d",&bookId) != 1) {
        printf("Invalid input for Book ID.\n");
        clear_input_buffer();
        return;
    }
    clear_input_buffer();

    // 1. Check if Book exists and is available using r+b for update
    FILE *fpBook = fopen("books.dat","r+b");
    if(fpBook==NULL)
        {
            printf("No books found to issue!\n"); return;
        }

    while(fread(&b,sizeof(b),1,fpBook))
        {
        if(b.id==bookId)
        {
            found=1;
            book_pos = ftell(fpBook) - sizeof(b); // Position to update
            tempB = b;
            break;
        }
    }

    if(!found)
        {
        printf("Book ID %d not found in the library!\n", bookId);
        fclose(fpBook);
        return;
        }

    if (tempB.quantity <= 0)
        {
        printf("Book '%s' is currently out of stock (Quantity: %d).\n", tempB.title, tempB.quantity);
        fclose(fpBook);
        return;
        }

    // 2. Decrease book quantity and update the file
    tempB.quantity--;
    fseek(fpBook, book_pos, SEEK_SET); // Go back to the record position
    fwrite(&tempB, sizeof(tempB), 1, fpBook); // Write the updated record

    fclose(fpBook);

    // 3. Record the issue
    printf("Enter Issue Date (DD/MM/YYYY): "); fgets(i.issueDate,sizeof(i.issueDate),stdin);
    i.issueDate[strcspn(i.issueDate,"\n")] = 0;

    printf("Enter Return Date (DD/MM/YYYY): "); fgets(i.returnDate,sizeof(i.returnDate),stdin);
    i.returnDate[strcspn(i.returnDate,"\n")] = 0;

    i.studentId = studentId;
    i.bookId = bookId;

    FILE *fpIssue = fopen("issues.dat","ab");
    if (fpIssue == NULL)
        {
        printf("Error opening issues file.\n");
        return;
        }
    fwrite(&i,sizeof(i),1,fpIssue);
    fclose(fpIssue);

    printf("Book '%s' issued successfully to Student ID %d! Remaining stock: %d\n", tempB.title, studentId, tempB.quantity);
}

// REGISTER BOOK RETURN SECTION (Quantity Update & Issue Record Removal Fixed)
void returnBook()
{
    int sId, bId, issue_found = 0, book_found = 0;
    struct Issue i;
    struct Book b;
    long book_pos = -1;

    printf("Enter Student ID for return: ");
    if (scanf("%d", &sId) != 1)
        {
        printf("Invalid input for Student ID.\n");
        clear_input_buffer();
        return;
        }
    clear_input_buffer(); // Input Buffer cleared after first scanf

    printf("Enter Book ID being returned: ");
    if (scanf("%d", &bId) != 1)
        {
        printf("Invalid input for Book ID.\n");
        clear_input_buffer();
        return;
        }
    clear_input_buffer(); // Input Buffer cleared after second scanf

    // 1. Update Book Quantity (Increase stock) using r+b
    FILE *fpBook = fopen("books.dat", "r+b");
    if (fpBook != NULL)
        {
        while(fread(&b, sizeof(b), 1, fpBook))
        {
            if (b.id == bId)
            {
                book_found = 1;
                book_pos = ftell(fpBook) - sizeof(b);
                b.quantity++; // Increase stock
                fseek(fpBook, book_pos, SEEK_SET);
                fwrite(&b, sizeof(b), 1, fpBook);
                break;
            }
        }
        fclose(fpBook);
    }

    // 2. Remove Issue Record
    FILE *fpIssue = fopen("issues.dat", "rb");
    if (fpIssue == NULL)
        {
        if(book_found) printf("Book stock updated, but no issue records were found to check.\n");
        return;
        }

    FILE *ft = fopen("temp_issues.dat", "wb");
    if (ft == NULL)
        {
        printf("Error creating temporary file for return.\n");
        fclose(fpIssue);
        return;
        }

    while(fread(&i, sizeof(i), 1, fpIssue))
        {
        // Find the specific issue record to remove
        if(i.studentId == sId && i.bookId == bId)
        {
            issue_found = 1;
            continue; // Skip writing the record being returned
        }
        fwrite(&i, sizeof(i), 1, ft); // Write all other records
    }

    fclose(fpIssue);
    fclose(ft);

    // File rename operations
    remove("issues.dat");
    rename("temp_issues.dat", "issues.dat");

    // Result feedback
    if (issue_found)
        {
        printf("Book ID %d returned by Student ID %d successfully! Stock updated.\n", bId, sId);
        }
    else
        {
        // iF SYSTEM DONOT FIND ANY issue record THEN IT WILL SHOW THIS MESSEGE
        printf("Issue record not found for Student ID %d and Book ID %d.\n", sId, bId);
        // If the stock increases but the record is not deleted, it will give a warning
        if(book_found) printf("Warning: Book stock was increased, but no matching issue record was deleted.\n");
    }
}
