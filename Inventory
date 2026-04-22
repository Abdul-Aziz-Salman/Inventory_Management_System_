/*
 * ============================================================
 *   INVENTORY MANAGEMENT SYSTEM USING BINARY SEARCH TREE (BST)
 * ============================================================
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_NAME 100
#define LOW_STOCK_THRESHOLD 5

/* ─── Data Structure ─────────────────────────────────────── */

typedef struct Product {
    int    id;
    char   name[MAX_NAME];
    float  price;
    int    quantity;
    struct Product *left;
    struct Product *right;
} Product;

/* ─── Node Creation ──────────────────────────────────────── */

Product* createNode(int id, const char *name, float price, int quantity) {
    Product *node = (Product*)malloc(sizeof(Product));
    if (!node) {
        printf("Memory allocation failed!\n");
        exit(1);
    }
    node->id       = id;
    strncpy(node->name, name, MAX_NAME - 1);
    node->name[MAX_NAME - 1] = '\0';
    node->price    = price;
    node->quantity = quantity;
    node->left     = NULL;
    node->right    = NULL;
    return node;
}

/* ─── Insert ─────────────────────────────────────────────── */

Product* insert(Product *root, int id, const char *name, float price, int quantity) {
    if (root == NULL)
        return createNode(id, name, price, quantity);

    if (id < root->id)
        root->left  = insert(root->left,  id, name, price, quantity);
    else if (id > root->id)
        root->right = insert(root->right, id, name, price, quantity);
    else
        printf("Product with ID %d already exists!\n", id);

    return root;
}

/* ─── Search ─────────────────────────────────────────────── */

Product* search(Product *root, int id) {
    if (root == NULL || root->id == id)
        return root;

    if (id < root->id)
        return search(root->left, id);
    else
        return search(root->right, id);
}

/* ─── Find Minimum Node (used in deletion) ───────────────── */

Product* findMin(Product *root) {
    while (root && root->left)
        root = root->left;
    return root;
}

/* ─── Delete ─────────────────────────────────────────────── */

Product* deleteNode(Product *root, int id) {
    if (root == NULL) {
        printf("Product with ID %d not found!\n", id);
        return NULL;
    }

    if (id < root->id) {
        root->left  = deleteNode(root->left,  id);
    } else if (id > root->id) {
        root->right = deleteNode(root->right, id);
    } else {
        /* Node found */
        if (root->left == NULL) {
            Product *temp = root->right;
            free(root);
            printf("Product deleted successfully.\n");
            return temp;
        } else if (root->right == NULL) {
            Product *temp = root->left;
            free(root);
            printf("Product deleted successfully.\n");
            return temp;
        } else {
            /* Two children: replace with inorder successor */
            Product *successor = findMin(root->right);
            root->id       = successor->id;
            strcpy(root->name, successor->name);
            root->price    = successor->price;
            root->quantity = successor->quantity;
            root->right    = deleteNode(root->right, successor->id);
        }
    }
    return root;
}

/* ─── Update ─────────────────────────────────────────────── */

void updateProduct(Product *root, int id) {
    Product *p = search(root, id);
    if (!p) {
        printf("Product with ID %d not found!\n", id);
        return;
    }
    int choice;
    printf("\nWhat to update?\n");
    printf("  1. Price\n");
    printf("  2. Quantity\n");
    printf("  3. Both\n");
    printf("Choice: ");
    scanf("%d", &choice);

    if (choice == 1 || choice == 3) {
        printf("New price: ");
        scanf("%f", &p->price);
    }
    if (choice == 2 || choice == 3) {
        printf("New quantity: ");
        scanf("%d", &p->quantity);
    }
    printf("Product updated successfully.\n");
}

/* ─── Display All (Inorder Traversal → sorted by ID) ─────── */

void displayAll(Product *root) {
    if (root == NULL) return;
    displayAll(root->left);
    printf("  %-6d %-25s %-10.2f %-10d\n",
           root->id, root->name, root->price, root->quantity);
    displayAll(root->right);
}

/* ─── Low Stock Alert ────────────────────────────────────── */

void lowStockAlert(Product *root) {
    if (root == NULL) return;
    lowStockAlert(root->left);
    if (root->quantity < LOW_STOCK_THRESHOLD)
        printf("  [LOW STOCK] ID: %-4d  %-25s  Qty: %d\n",
               root->id, root->name, root->quantity);
    lowStockAlert(root->right);
}

/* ─── Find Max Price Product ─────────────────────────────── */

void findMaxPrice(Product *root, Product **maxNode) {
    if (root == NULL) return;
    if (*maxNode == NULL || root->price > (*maxNode)->price)
        *maxNode = root;
    findMaxPrice(root->left,  maxNode);
    findMaxPrice(root->right, maxNode);
}

/* ─── Find Min Price Product ─────────────────────────────── */

void findMinPrice(Product *root, Product **minNode) {
    if (root == NULL) return;
    if (*minNode == NULL || root->price < (*minNode)->price)
        *minNode = root;
    findMinPrice(root->left,  minNode);
    findMinPrice(root->right, minNode);
}

/* ─── Count Total Nodes ──────────────────────────────────── */

int countNodes(Product *root) {
    if (root == NULL) return 0;
    return 1 + countNodes(root->left) + countNodes(root->right);
}

/* ─── Free All Memory ────────────────────────────────────── */

void freeTree(Product *root) {
    if (root == NULL) return;
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}

/* ─── Print Table Header ─────────────────────────────────── */

void printHeader() {
    printf("\n  %-6s %-25s %-10s %-10s\n", "ID", "Name", "Price", "Quantity");
    printf("  %s\n", "------------------------------------------------------------");
}

/* ─── Main Menu ──────────────────────────────────────────── */

void printMenu() {
    printf("\n+======================================+\n");
    printf("|   INVENTORY MANAGEMENT SYSTEM (BST)  |\n");
    printf("+======================================+\n");
    printf("|  1. Add Product                      |\n");
    printf("|  2. Search Product                   |\n");
    printf("|  3. Delete Product                   |\n");
    printf("|  4. Update Product                   |\n");
    printf("|  5. Display All Products             |\n");
    printf("|  6. Low Stock Alert                  |\n");
    printf("|  7. Most Expensive Product           |\n");
    printf("|  8. Cheapest Product                 |\n");
    printf("|  9. Total Products Count             |\n");
    printf("|  0. Exit                             |\n");
    printf("+======================================+\n");
    printf("Enter choice: ");
}

/* ─── Main ───────────────────────────────────────────────── */

int main() {
    Product *root = NULL;

    /* Pre-load sample data */
    root = insert(root, 101, "Laptop",          75000.00, 10);
    root = insert(root, 205, "Wireless Mouse",    850.00,  3);
    root = insert(root, 312, "USB-C Hub",        2200.00,  8);
    root = insert(root, 150, "Mechanical Keyboard",4500.00, 2);
    root = insert(root, 400, "Monitor 24\"",    18000.00, 12);
    root = insert(root, 275, "Webcam HD",        3200.00,  4);

    int  choice;
    int  id, quantity;
    char name[MAX_NAME];
    float price;

    do {
        printMenu();
        scanf("%d", &choice);

        switch (choice) {

        case 1: /* Add */
            printf("Enter Product ID    : ");
            scanf("%d", &id);
            printf("Enter Product Name  : ");
            getchar();
            fgets(name, MAX_NAME, stdin);
            name[strcspn(name, "\n")] = '\0';
            printf("Enter Price (BDT)   : ");
            scanf("%f", &price);
            printf("Enter Quantity      : ");
            scanf("%d", &quantity);
            root = insert(root, id, name, price, quantity);
            printf("Product added successfully.\n");
            break;

        case 2: /* Search */
            printf("Enter Product ID to search: ");
            scanf("%d", &id);
            {
                Product *found = search(root, id);
                if (found) {
                    printHeader();
                    printf("  %-6d %-25s %-10.2f %-10d\n",
                           found->id, found->name, found->price, found->quantity);
                } else {
                    printf("Product with ID %d not found.\n", id);
                }
            }
            break;

        case 3: /* Delete */
            printf("Enter Product ID to delete: ");
            scanf("%d", &id);
            root = deleteNode(root, id);
            break;

        case 4: /* Update */
            printf("Enter Product ID to update: ");
            scanf("%d", &id);
            updateProduct(root, id);
            break;

        case 5: /* Display All */
            if (root == NULL) {
                printf("Inventory is empty.\n");
            } else {
                printHeader();
                displayAll(root);
            }
            break;

        case 6: /* Low Stock */
            printf("\n--- Low Stock Products (Qty < %d) ---\n", LOW_STOCK_THRESHOLD);
            lowStockAlert(root);
            break;

        case 7: /* Most expensive */
            {
                Product *maxNode = NULL;
                findMaxPrice(root, &maxNode);
                if (maxNode) {
                    printHeader();
                    printf("  %-6d %-25s %-10.2f %-10d\n",
                           maxNode->id, maxNode->name, maxNode->price, maxNode->quantity);
                }
            }
            break;

        case 8: /* Cheapest */
            {
                Product *minNode = NULL;
                findMinPrice(root, &minNode);
                if (minNode) {
                    printHeader();
                    printf("  %-6d %-25s %-10.2f %-10d\n",
                           minNode->id, minNode->name, minNode->price, minNode->quantity);
                }
            }
            break;

        case 9: /* Count */
            printf("Total products in inventory: %d\n", countNodes(root));
            break;

        case 0:
            printf("Exiting... Goodbye!\n");
            break;

        default:
            printf("Invalid choice. Please try again.\n");
        }

    } while (choice != 0);

    freeTree(root);
    return 0;
}
