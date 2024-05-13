1.	Write a Program to create a Binary Tree and perform following nonrecursive operations on it. a. Preorder Traversal, b. Postorder Traversal, c. Count total no. of nodes, d. Display height of a tree.



#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* left;
    struct Node* right;
};

struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    return newNode;
}

void preorderTraversal(struct Node* root) {
    if (root) {
        printf("%d ", root->data);
        struct Node* stack[100];
        int top = -1;
        stack[++top] = root;
        while (top > -1) {
            struct Node* current = stack[top--];
            if (current->right) {
                stack[++top] = current->right;
            }
            if (current->left) {
                stack[++top] = current->left;
            }
        }
    }
}

void inorderTraversal(struct Node* root) {
    if (root) {
        struct Node* stack[100];
        int top = -1;
        struct Node* current = root;
        while (current || top > -1) {
            if (current) {
                stack[++top] = current;
                current = current->left;
            } else {
                current = stack[top--];
                printf("%d ", current->data);
                current = current->right;
            }
        }
    }
}

void postorderTraversal(struct Node* root) {
    if (root) {
        struct Node* stack[100];
        int top = -1;
        struct Node* current = root;
        struct Node* lastVisited = NULL;
        while (current || top > -1) {
            if (current) {
                stack[++top] = current;
                current = current->left;
            } else {
                current = stack[top--];
                if (!current->right || current->right == lastVisited) {
                    printf("%d ", current->data);
                    lastVisited = current;
                } else {
                    current = current->right;
                }
            }
        }
    }
}

int countNodes(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    return 1 + countNodes(root->left) + countNodes(root->right);
}

int calculateHeight(struct Node* root) {
    if (root == NULL) {
        return -1;
    }
    return 1 + max(calculateHeight(root->left), calculateHeight(root->right));
}

int main() {
    struct Node* root = createNode(1);
    root->left = createNode(2);
    root->right = createNode(3);
    root->left->left = createNode(4);
    root->left->right = createNode(5);

    printf("Preorder traversal: ");
    preorderTraversal(root);
    printf("\n");

    printf("Inorder traversal: ");
    inorderTraversal(root);
    printf("\n");

    printf("Postorder traversal: ");
    postorderTraversal(root);
    printf("\n");

    printf("Total number of nodes: %d\n", countNodes(root));

    printf("Height of the tree: %d\n", calculateHeight(root));

    return 0;
}



2.	Write a Program to create a Binary Tree and perform following nonrecursive operations on it. a. inorder Traversal; b. Count no. of nodes on longest path; c. display tree levelwise; d. Display height of a tree.


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to perform inorder traversal non-recursively
void inorderTraversal(struct TreeNode* root) {
    // Stack to store nodes
    struct TreeNode* stack[100];
    int top = -1;
    struct TreeNode* current = root;

    while (current != NULL || top != -1) {
        while (current != NULL) {
            stack[++top] = current;
            current = current->left;
        }
        current = stack[top--];
        printf("%d ", current->data);
        current = current->right;
    }
}

// Function to count the number of nodes on the longest path
int countNodesOnLongestPath(struct TreeNode* root) {
    if (root == NULL) return 0;

    // Use level order traversal to count nodes on the longest path
    int count = 0;
    struct TreeNode* queue[100];
    int front = 0, rear = -1;
    queue[++rear] = root;

    while (front <= rear) {
        int levelNodes = rear - front + 1;
        if (levelNodes > 0) {
            count = levelNodes;
            while (levelNodes > 0) {
                struct TreeNode* temp = queue[front++];
                if (temp->left != NULL) queue[++rear] = temp->left;
                if (temp->right != NULL) queue[++rear] = temp->right;
                levelNodes--;
            }
        }
    }

    return count;
}

// Function to display the tree level-wise
void displayLevelWise(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* queue[100];
    int front = 0, rear = -1;
    queue[++rear] = root;

    while (front <= rear) {
        int levelNodes = rear - front + 1;
        while (levelNodes > 0) {
            struct TreeNode* temp = queue[front++];
            printf("%d ", temp->data);
            if (temp->left != NULL) queue[++rear] = temp->left;
            if (temp->right != NULL) queue[++rear] = temp->right;
            levelNodes--;
        }
        printf("\n");
    }
}

// Function to calculate the height of a tree
int heightOfTree(struct TreeNode* root) {
    if (root == NULL) return -1;

    struct TreeNode* queue[100];
    int front = 0, rear = -1;
    queue[++rear] = root;
    int height = -1;

    while (front <= rear) {
        int levelNodes = rear - front + 1;
        while (levelNodes > 0) {
            struct TreeNode* temp = queue[front++];
            if (temp->left != NULL) queue[++rear] = temp->left;
            if (temp->right != NULL) queue[++rear] = temp->right;
            levelNodes--;
        }
        height++;
    }

    return height;
}

int main() {
    // Creating a binary tree
    struct TreeNode* root = createNode(1);
    root->left = createNode(2);
    root->right = createNode(3);
    root->left->left = createNode(4);
    root->left->right = createNode(5);
    root->right->left = createNode(6);
    root->right->right = createNode(7);

    printf("Inorder Traversal: ");
    inorderTraversal(root);
    printf("\n");

    printf("Number of nodes on longest path: %d\n", countNodesOnLongestPath(root));

    printf("Tree displayed level-wise:\n");
    displayLevelWise(root);

    printf("Height of the tree: %d\n", heightOfTree(root));

    return 0;
}





3.	Write a Program to create a Binary Search Tree holding numeric keys and perform following nonrecursive operations on it. a. Levelwise display, b. Mirror image, c. Display height of a tree, d. Find 


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    struct TreeNode* current = root;
    struct TreeNode* parent = NULL;

    while (current != NULL) {
        parent = current;
        if (key < current->data) {
            current = current->left;
        } else {
            current = current->right;
        }
    }

    if (key < parent->data) {
        parent->left = createNode(key);
    } else {
        parent->right = createNode(key);
    }

    return root;
}

// Function to perform level-wise display of the BST
void levelWiseDisplay(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* queue[100];
    int front = 0, rear = -1;
    queue[++rear] = root;

    while (front <= rear) {
        int levelNodes = rear - front + 1;
        while (levelNodes > 0) {
            struct TreeNode* temp = queue[front++];
            printf("%d ", temp->data);
            if (temp->left != NULL) queue[++rear] = temp->left;
            if (temp->right != NULL) queue[++rear] = temp->right;
            levelNodes--;
        }
        printf("\n");
    }
}

// Function to create mirror image of a binary tree
void mirrorImage(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* current = root;
    struct TreeNode* temp;

    // Swap left and right subtrees
    while (current != NULL) {
        temp = current->left;
        current->left = current->right;
        current->right = temp;

        if (current->left != NULL) mirrorImage(current->left);
        if (current->right != NULL) mirrorImage(current->right);

        current = current->left;
    }
}

// Function to calculate the height of a BST
int heightOfBST(struct TreeNode* root) {
    if (root == NULL) return -1;

    struct TreeNode* queue[100];
    int front = 0, rear = -1;
    queue[++rear] = root;
    int height = -1;

    while (front <= rear) {
        int levelNodes = rear - front + 1;
        while (levelNodes > 0) {
            struct TreeNode* temp = queue[front++];
            if (temp->left != NULL) queue[++rear] = temp->left;
            if (temp->right != NULL) queue[++rear] = temp->right;
            levelNodes--;
        }
        height++;
    }

    return height;
}

// Function to search for a key in the BST
struct TreeNode* searchBST(struct TreeNode* root, int key) {
    struct TreeNode* current = root;

    while (current != NULL) {
        if (key == current->data) {
            return current;
        } else if (key < current->data) {
            current = current->left;
        } else {
            current = current->right;
        }
    }

    return NULL;
}

int main() {
    struct TreeNode* root = NULL;

    // Inserting elements into the BST
    root = insertNode(root, 50);
    root = insertNode(root, 30);
    root = insertNode(root, 70);
    root = insertNode(root, 20);
    root = insertNode(root, 40);
    root = insertNode(root, 60);
    root = insertNode(root, 80);

    printf("BST Level-wise Display:\n");
    levelWiseDisplay(root);

    printf("\nMirror Image of the BST:\n");
    mirrorImage(root);
    levelWiseDisplay(root);

    printf("\nHeight of the BST: %d\n", heightOfBST(root));

    int keyToFind = 40;
    struct TreeNode* foundNode = searchBST(root, keyToFind);
    if (foundNode != NULL) {
        printf("\nFound key %d in the BST.\n", keyToFind);
    } else {
        printf("\nKey %d not found in the BST.\n", keyToFind);
    }

    return 0;
}



4.	Write a program to illustrate operations on a BST holding numeric keys. The menu must include: • Insert • Delete • Find • display in Inorder way


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to find the minimum value node in a BST
struct TreeNode* minValueNode(struct TreeNode* node) {
    struct TreeNode* current = node;
    while (current && current->left != NULL) {
        current = current->left;
    }
    return current;
}

// Function to delete a node from the BST
struct TreeNode* deleteNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return root;
    }

    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    } else {
        if (root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }

        struct TreeNode* temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}

// Function to search for a key in the BST
struct TreeNode* searchBST(struct TreeNode* root, int key) {
    if (root == NULL || root->data == key) {
        return root;
    }

    if (root->data < key) {
        return searchBST(root->right, key);
    }

    return searchBST(root->left, key);
}

// Function to perform inorder traversal of the BST
void inorderTraversal(struct TreeNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->data);
        inorderTraversal(root->right);
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Delete\n");
        printf("3. Find\n");
        printf("4. Display Inorder\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Enter key to delete: ");
                scanf("%d", &key);
                root = deleteNode(root, key);
                break;
            case 3:
                printf("Enter key to find: ");
                scanf("%d", &key);
                if (searchBST(root, key) != NULL) {
                    printf("Key %d found in the BST.\n", key);
                } else {
                    printf("Key %d not found in the BST.\n", key);
                }
                break;
            case 4:
                printf("Inorder Traversal: ");
                inorderTraversal(root);
                printf("\n");
                break;
            case 5:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 5);

    return 0;
}



5.	Write a program to illustrate operations on a BST holding numeric keys. The menu must include: • Insert • Mirror Image • Find • Post order (nonrecursive)


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to create mirror image of a binary tree
void mirrorImage(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* current = root;
    struct TreeNode* temp;

    // Swap left and right subtrees
    while (current != NULL) {
        temp = current->left;
        current->left = current->right;
        current->right = temp;

        if (current->left != NULL) mirrorImage(current->left);
        if (current->right != NULL) mirrorImage(current->right);

        current = current->left;
    }
}

// Function to search for a key in the BST
struct TreeNode* searchBST(struct TreeNode* root, int key) {
    if (root == NULL || root->data == key) {
        return root;
    }

    if (root->data < key) {
        return searchBST(root->right, key);
    }

    return searchBST(root->left, key);
}

// Function to perform post-order traversal of the BST (non-recursive)
void postOrderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    struct TreeNode* current = root;
    struct TreeNode* lastVisitedNode = NULL;

    while (current != NULL || top != -1) {
        while (current != NULL) {
            stack[++top] = current;
            current = current->left;
        }

        struct TreeNode* peekNode = stack[top];

        if (peekNode->right != NULL && lastVisitedNode != peekNode->right) {
            current = peekNode->right;
        } else {
            printf("%d ", peekNode->data);
            lastVisitedNode = stack[top--];
        }
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Mirror Image\n");
        printf("3. Find\n");
        printf("4. Post-order Traversal\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Creating mirror image...\n");
                mirrorImage(root);
                break;
            case 3:
                printf("Enter key to find: ");
                scanf("%d", &key);
                if (searchBST(root, key) != NULL) {
                    printf("Key %d found in the BST.\n", key);
                } else {
                    printf("Key %d not found in the BST.\n", key);
                }
                break;
            case 4:
                printf("Post-order Traversal: ");
                postOrderTraversal(root);
                printf("\n");
                break;
            case 5:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 5);

    return 0;
}



6.	Write a Program to create a Binary Search Tree and perform following nonrecursive operations on it. a. Preorder Traversal b. Inorder Traversal c. Display Number of Leaf Nodes d. Mirror Image


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to perform preorder traversal of the BST (non-recursive)
void preorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    stack[++top] = root;

    while (top != -1) {
        struct TreeNode* current = stack[top--];
        printf("%d ", current->data);

        if (current->right != NULL) stack[++top] = current->right;
        if (current->left != NULL) stack[++top] = current->left;
    }
}

// Function to perform inorder traversal of the BST (non-recursive)
void inorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    struct TreeNode* current = root;

    while (current != NULL || top != -1) {
        while (current != NULL) {
            stack[++top] = current;
            current = current->left;
        }

        current = stack[top--];
        printf("%d ", current->data);
        current = current->right;
    }
}

// Function to count the number of leaf nodes in the BST
int countLeafNodes(struct TreeNode* root) {
    if (root == NULL) return 0;

    int count = 0;
    struct TreeNode* stack[100];
    int top = -1;
    stack[++top] = root;

    while (top != -1) {
        struct TreeNode* current = stack[top--];
        if (current->left == NULL && current->right == NULL) {
            count++;
        }
        if (current->right != NULL) stack[++top] = current->right;
        if (current->left != NULL) stack[++top] = current->left;
    }

    return count;
}

// Function to create mirror image of a binary tree
void mirrorImage(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* current = root;
    struct TreeNode* temp;

    // Swap left and right subtrees
    while (current != NULL) {
        temp = current->left;
        current->left = current->right;
        current->right = temp;

        if (current->left != NULL) mirrorImage(current->left);
        if (current->right != NULL) mirrorImage(current->right);

        current = current->left;
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Preorder Traversal\n");
        printf("3. Inorder Traversal\n");
        printf("4. Count Leaf Nodes\n");
        printf("5. Mirror Image\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Preorder Traversal: ");
                preorderTraversal(root);
                printf("\n");
                break;
            case 3:
                printf("Inorder Traversal: ");
                inorderTraversal(root);
                printf("\n");
                break;
            case 4:
                printf("Number of Leaf Nodes: %d\n", countLeafNodes(root));
                break;
            case 5:
                printf("Creating mirror image...\n");
                mirrorImage(root);
                break;
            case 6:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 6);

    return 0;
}



7.	Write a Program to create a Binary Search Tree and perform following nonrecursive operations on it. a. Preorder Traversal b. Postorder Traversal c. Display total Number of Nodes d. Display Leaf nodes.

#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to perform preorder traversal of the BST (non-recursive)
void preorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    stack[++top] = root;

    while (top != -1) {
        struct TreeNode* current = stack[top--];
        printf("%d ", current->data);

        if (current->right != NULL) stack[++top] = current->right;
        if (current->left != NULL) stack[++top] = current->left;
    }
}

// Function to perform postorder traversal of the BST (non-recursive)
void postorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack1[100];
    struct TreeNode* stack2[100];
    int top1 = -1;
    int top2 = -1;
    stack1[++top1] = root;

    while (top1 != -1) {
        struct TreeNode* current = stack1[top1--];
        stack2[++top2] = current;

        if (current->left != NULL) stack1[++top1] = current->left;
        if (current->right != NULL) stack1[++top1] = current->right;
    }

    while (top2 != -1) {
        printf("%d ", stack2[top2--]->data);
    }
}

// Function to count the total number of nodes in the BST
int countNodes(struct TreeNode* root) {
    if (root == NULL) return 0;

    int count = 0;
    struct TreeNode* stack[100];
    int top = -1;
    stack[++top] = root;

    while (top != -1) {
        struct TreeNode* current = stack[top--];
        count++;

        if (current->right != NULL) stack[++top] = current->right;
        if (current->left != NULL) stack[++top] = current->left;
    }

    return count;
}

// Function to display leaf nodes of the BST
void displayLeafNodes(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    stack[++top] = root;

    while (top != -1) {
        struct TreeNode* current = stack[top--];
        if (current->left == NULL && current->right == NULL) {
            printf("%d ", current->data);
        }
        if (current->right != NULL) stack[++top] = current->right;
        if (current->left != NULL) stack[++top] = current->left;
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Preorder Traversal\n");
        printf("3. Postorder Traversal\n");
        printf("4. Display Total Number of Nodes\n");
        printf("5. Display Leaf Nodes\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Preorder Traversal: ");
                preorderTraversal(root);
                printf("\n");
                break;
            case 3:
                printf("Postorder Traversal: ");
                postorderTraversal(root);
                printf("\n");
                break;
            case 4:
                printf("Total Number of Nodes: %d\n", countNodes(root));
                break;
            case 5:
                printf("Leaf Nodes: ");
                displayLeafNodes(root);
                printf("\n");
                break;
            case 6:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 6);

    return 0;
}



8.	Write a Program to create a Binary Search Tree and perform deletion of a node from it. Also display the tree in nonrecursive postorder way.


#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to find the minimum value node in a BST
struct TreeNode* minValueNode(struct TreeNode* node) {
    struct TreeNode* current = node;
    while (current && current->left != NULL) {
        current = current->left;
    }
    return current;
}

// Function to perform deletion of a node from the BST
struct TreeNode* deleteNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return root;
    }

    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    } else {
        if (root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }

        struct TreeNode* temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}

// Function to display the tree in non-recursive postorder traversal
void postorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;

    struct TreeNode* stack[100];
    int top = -1;
    struct TreeNode* current = root;
    struct TreeNode* lastVisitedNode = NULL;

    while (current != NULL || top != -1) {
        while (current != NULL) {
            stack[++top] = current;
            current = current->left;
        }

        struct TreeNode* peekNode = stack[top];

        if (peekNode->right != NULL && lastVisitedNode != peekNode->right) {
            current = peekNode->right;
        } else {
            printf("%d ", peekNode->data);
            lastVisitedNode = stack[top--];
        }
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Delete\n");
        printf("3. Display Tree in Non-recursive Postorder\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Enter key to delete: ");
                scanf("%d", &key);
                root = deleteNode(root, key);
                break;
            case 3:
                printf("Tree in Non-recursive Postorder: ");
                postorderTraversal(root);
                printf("\n");
                break;
            case 4:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 4);

    return 0;
}



9.	Write a Program to create a Binary Search Tree and display it levelwise. Also perform deletion of a node from it.



#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to find the minimum value node in a BST
struct TreeNode* minValueNode(struct TreeNode* node) {
    struct TreeNode* current = node;
    while (current && current->left != NULL) {
        current = current->left;
    }
    return current;
}

// Function to perform deletion of a node from the BST
struct TreeNode* deleteNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return root;
    }

    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    } else {
        if (root->left == NULL) {
            struct TreeNode* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct TreeNode* temp = root->left;
            free(root);
            return temp;
        }

        struct TreeNode* temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}

// Function to find the height of the tree
int height(struct TreeNode* root) {
    if (root == NULL) {
        return 0;
    } else {
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        if (leftHeight > rightHeight) {
            return (leftHeight + 1);
        } else {
            return (rightHeight + 1);
        }
    }
}

// Function to print nodes at a given level
void printLevel(struct TreeNode* root, int level) {
    if (root == NULL) {
        return;
    }
    if (level == 1) {
        printf("%d ", root->data);
    } else if (level > 1) {
        printLevel(root->left, level - 1);
        printLevel(root->right, level - 1);
    }
}

// Function to print the tree level-wise
void printLevelOrder(struct TreeNode* root) {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
        printf("Level %d: ", i);
        printLevel(root, i);
        printf("\n");
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Delete\n");
        printf("3. Display Level-wise\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Enter key to delete: ");
                scanf("%d", &key);
                root = deleteNode(root, key);
                break;
            case 3:
                printf("Tree Level-wise:\n");
                printLevelOrder(root);
                break;
            case 4:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 4);

    return 0;
}




10.	Write a Program to create a Binary Search Tree and display its mirror image with and without disturbing the original tree. Also display height of a tree using nonrecursion.

#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
};

// Function to create a new tree node
struct TreeNode* createNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a new node into the BST
struct TreeNode* insertNode(struct TreeNode* root, int key) {
    if (root == NULL) {
        return createNode(key);
    }

    if (key < root->data) {
        root->left = insertNode(root->left, key);
    } else if (key > root->data) {
        root->right = insertNode(root->right, key);
    }

    return root;
}

// Function to calculate the height of the tree using non-recursive method
int height(struct TreeNode* root) {
    if (root == NULL) {
        return 0;
    }

    int level = 0;
    struct TreeNode* queue[100];
    int front = 0, rear = 0;
    queue[rear++] = root;

    while (front < rear) {
        int count = rear - front;
        while (count--) {
            struct TreeNode* temp = queue[front++];
            if (temp->left != NULL) {
                queue[rear++] = temp->left;
            }
            if (temp->right != NULL) {
                queue[rear++] = temp->right;
            }
        }
        level++;
    }

    return level;
}

// Function to create mirror image of a binary tree without disturbing the original tree
struct TreeNode* mirrorImage(struct TreeNode* root) {
    if (root == NULL) {
        return NULL;
    }

    struct TreeNode* mirror = createNode(root->data);
    mirror->left = mirrorImage(root->right);
    mirror->right = mirrorImage(root->left);

    return mirror;
}

// Function to display inorder traversal of the tree
void inorderTraversal(struct TreeNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->data);
        inorderTraversal(root->right);
    }
}

// Function to display level-wise of the tree
void printLevelOrder(struct TreeNode* root) {
    int h = height(root);
    for (int i = 1; i <= h; i++) {
        printf("Level %d: ", i);
        printLevel(root, i);
        printf("\n");
    }
}

// Function to print nodes at a given level
void printLevel(struct TreeNode* root, int level) {
    if (root == NULL) {
        return;
    }
    if (level == 1) {
        printf("%d ", root->data);
    } else if (level > 1) {
        printLevel(root->left, level - 1);
        printLevel(root->right, level - 1);
    }
}

int main() {
    struct TreeNode* root = NULL;
    int choice, key;

    do {
        printf("\nMenu:\n");
        printf("1. Insert\n");
        printf("2. Display Original Tree\n");
        printf("3. Display Mirror Image Tree\n");
        printf("4. Display Height of Tree\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key to insert: ");
                scanf("%d", &key);
                root = insertNode(root, key);
                break;
            case 2:
                printf("Original Tree (Inorder Traversal): ");
                inorderTraversal(root);
                printf("\n");
                break;
            case 3:
                printf("Mirror Image Tree (Inorder Traversal): ");
                struct TreeNode* mirror = mirrorImage(root);
                inorderTraversal(mirror);
                printf("\n");
                break;
            case 4:
                printf("Height of Tree: %d\n", height(root));
                break;
            case 5:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid choice.\n");
        }
    } while (choice != 5);

    return 0;
}
