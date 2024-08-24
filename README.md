# demo
This is a demmo that i create just to learn github.
//simple chatbot using c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct Node {
    char message[256];
    struct Node* next;
} Node;
typedef struct LinkedList {
    Node* head;
} LinkedList;
Node* create_node(char* message) {
    Node* new_node = (Node*)malloc(sizeof(Node));
    strcpy(new_node->message, message);
    new_node->next = NULL;
    return new_node;
}
void append(LinkedList* list, char* message) {
    Node* new_node = create_node(message);
    if (list->head == NULL) {
        list->head = new_node;
    } else {
        Node* current = list->head;
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = new_node;
    }
}
void print_list(LinkedList* list) {
    Node* current = list->head;
    while (current != NULL) {
        printf("%s\n", current->message);
        current = current->next;
    }
}
typedef struct Chatbot {
    LinkedList* conversation_history;
    char* responses[256];
} Chatbot;
Chatbot* create_chatbot() {
    Chatbot* chatbot = (Chatbot*)malloc(sizeof(Chatbot));
    chatbot->conversation_history = (LinkedList*)malloc(sizeof(LinkedList));
    chatbot->conversation_history->head = NULL;
    chatbot->responses[0] = "hello";
    chatbot->responses[1] = "Hi! How are you?";
    chatbot->responses[2] = "how are you";
    chatbot->responses[3] = "I'm good, thanks! How about you?";
    chatbot->responses[4] = "what's your name";
    chatbot->responses[5] = "My name is Chatbot!";
    chatbot->responses[6] = "quit";
    chatbot->responses[7] = "Goodbye!";
    return chatbot;
}
char* respond(Chatbot* chatbot, char* user_input) {
    int i;
    for (i = 0; i < 8; i += 2) {
        if (strcmp(user_input, chatbot->responses[i]) == 0) {
            char* response = chatbot->responses[i + 1];
            char user_message[256];
            sprintf(user_message, "User: %s", user_input);
            append(chatbot->conversation_history, user_message);
            char chatbot_message[256];
            sprintf(chatbot_message, "Chatbot: %s", response);
            append(chatbot->conversation_history, chatbot_message);
            return response;
        }
    }
    return "I didn't understand that. Please try again!";
}


void view_history(Chatbot* chatbot) {
    print_list(chatbot->conversation_history);
}


int main() {
    Chatbot* chatbot = create_chatbot();
    while (1) {
        char user_input[256];
        printf("User: ");
        fgets(user_input, 256, stdin);
        user_input[strlen(user_input) - 1] = '\0';
        if (strcmp(user_input, "quit") == 0) {
            break;
        }
        char* response = respond(chatbot, user_input);
        printf("Chatbot: %s\n", response);
        if (strcmp(user_input, "view history") == 0) {
            view_history(chatbot);
        }
    }
    return 0;
}
