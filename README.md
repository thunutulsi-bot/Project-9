#include <stdio.h>
#include <stringstring.h>

#define MAX 100

struct Contact {
    char name[50];
    char phone[20];
};

void addContact(struct Contact contacts[], int *count) {
    if (*count >= MAX) {
        printf("Contact list is full!\n");
        return;
    }

    printf("Enter name: ");
    getchar(); 
    fgets(contacts[*count].name, 50, stdin);
    contacts[*count].name[strcspn(contacts[*count].name, "\n")] = '\0';

    printf("Enter phone number: ");
    fgets(contacts[*count].phone, 20, stdin);
    contacts[*count].phone[strcspn(contacts[*count].phone, "\n")] = '\0';

    (*count)++;
    printf("Contact added successfully!\n");
}

void viewContacts(struct Contact contacts[], int count) {
    if (count == 0) {
        printf("No contacts found.\n");
        return;
    }

    printf("\n---- Contact List ----\n");
    for (int i = 0; i < count; i++) {
        printf("%d. Name: %s | Phone: %s\n", i + 1, contacts[i].name, contacts[i].phone);
    }
}

void searchContact(struct Contact contacts[], int count) {
    if (count == 0) {
        printf("No contacts available.\n");
        return;
    }

    char searchName[50];
    printf("Enter name to search: ");
    getchar();
    fgets(searchName, 50, stdin);
    searchName[strcspn(searchName, "\n")] = '\0';

    for (int i = 0; i < count; i++) {
        if (strcmp(contacts[i].name, searchName) == 0) {
            printf("Contact Found:\nName: %s\nPhone: %s\n", contacts[i].name, contacts[i].phone);
            return;
        }
    }

    printf("Contact not found.\n");
}

void deleteContact(struct Contact contacts[], int *count) {
    if (*count == 0) {
        printf("No contacts to delete.\n");
        return;
    }

    int index;
    viewContacts(contacts, *count);
    printf("Enter contact number to delete: ");
    scanf("%d", &index);

    if (index < 1 || index > *count) {
        printf("Invalid contact number.\n");
        return;
    }

    for (int i = index - 1; i < *count - 1; i++) {
        contacts[i] = contacts[i + 1];
    }

    (*count)--;
    printf("Contact deleted successfully!\n");
}

int main() {
    struct Contact contacts[MAX];
    int count = 0;
    int choice;

    while (1) {
        printf("\n===== CONTACT BOOK MENU =====\n");
        printf("1. Add Contact\n");
        printf("2. View Contacts\n");
        printf("3. Search Contact\n");
        printf("4. Delete Contact\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addContact(contacts, &count);
                break;
            case 2:
                viewContacts(contacts, count);
                break;
            case 3:
                searchContact(contacts, count);
                break;
            case 4:
                deleteContact(contacts, &count);
                break;
            case 5:
                printf("Exiting Contact Book... Goodbye!\n");
                return 0;
            default:
                printf("Invalid choice! Try again.\n");
        }
    }
}
