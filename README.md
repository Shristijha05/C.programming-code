#include <stdio.h>
#include <string.h>

// Define a structure to store user information
struct User {
    char username[50];
    char password[50];
    int hasVoted; // 0 for not voted, 1 for voted
};

// Define an array to store user data (In practice, you would use a database)
struct User users[10];

// Define the available voting choices
char votingChoices[][100] = {
    "Choice 1: Candidate A",
    "Choice 2: Candidate B",
    "Choice 3: Candidate C"
};

int numChoices = 3; // Number of available choices

// Function to register a new user
void registerUser(int *userCount) {
    if (*userCount >= 10) {
        printf("Error: Maximum number of users reached.\n");
        return;
    }

    printf("Enter username: ");
    scanf("%s", users[*userCount].username);
    printf("Enter password: ");
    scanf("%s", users[*userCount].password);
    users[*userCount].hasVoted = 0;

    (*userCount)++;
    printf("User registered successfully.\n");
}

// Function to authenticate a user
int authenticateUser(char *username, char *password, int userCount) {
    for (int i = 0; i < userCount; i++) {
        if (strcmp(username, users[i].username) == 0 && strcmp(password, users[i].password) == 0) {
            return i; // Authentication successful, return the user index
        }
    }
    return -1; // Authentication failed
}

// Function to display voting choices
void displayVotingChoices() {
    printf("\nVoting Choices:\n");
    for (int i = 0; i < numChoices; i++) {
        printf("%d. %s\n", i + 1, votingChoices[i]);
    }
}

int main() {
    int userCount = 0;
    int choice;

    do {
        printf("\nOnline Voting System\n");
        printf("1. Register\n");
        printf("2. Login\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                registerUser(&userCount);
                break;
            case 2:
                {
                    char username[50];
                    char password[50];
                    int userIndex;
                    printf("Enter username: ");
                    scanf("%s", username);
                    printf("Enter password: ");
                    scanf("%s", password);

                    userIndex = authenticateUser(username, password, userCount);

                    if (userIndex != -1) {
                        if (users[userIndex].hasVoted) {
                            printf("You have already voted.\n");
                        } else {
                            displayVotingChoices();
                            int vote;
                            printf("Enter your vote (1-%d): ", numChoices);
                            scanf("%d", &vote);

                            if (vote >= 1 && vote <= numChoices) {
                                printf("Thank you for voting. You voted for: %s\n", votingChoices[vote - 1]);
                                users[userIndex].hasVoted = 1;
                            } else {
                                printf("Invalid vote choice.\n");
                            }
                        }
                    } else {
                        printf("Login failed. Invalid username or password.\n");
                    }
                }
                break;
            case 3:
                printf("Exiting the program.\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                break;
        }
    } while (choice != 3);

    return 0;
}
