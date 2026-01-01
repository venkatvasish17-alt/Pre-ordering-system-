#include <stdio.h>
#include <string.h>

struct Customer
{
    char name[50];
    int qty[20];
    float total;
};

/* FUNCTION DECLARATIONS */
void inputMenu(int n, char dish[][50], float price[]);
void displayMenu(int n, char dish[][50], float price[]);
struct Customer takeOrder(struct Customer c, int n, float price[]);
void printBill(char restname[], struct Customer c, int n, char dish[][50], float price[]);

/* FUNCTION DEFINITIONS */

void inputMenu(int n, char dish[][50], float price[])
{
    int i;
    for (i = 0; i < n; i++)
    {
        printf("ENTER DISH %d NAME: ", i + 1);
        scanf("%s", dish[i]);

        printf("ENTER PRICE OF %s: ", dish[i]);
        scanf("%f", &price[i]);
    }
}

void displayMenu(int n, char dish[][50], float price[])
{
    int i;
    printf("\n------ MENU ------\n");
    for (i = 0; i < n; i++)
        printf("%d. %s - %.2f\n", i + 1, dish[i], price[i]);
}

struct Customer takeOrder(struct Customer c, int n, float price[])
{
    int i, choice, qty;

    c.total = 0;
    for (i = 0; i < n; i++)
        c.qty[i] = 0;

    while (1)
    {
        printf("ENTER DISH NUMBER (0 TO FINISH): ");
        scanf("%d", &choice);

        if (choice == 0)
            break;

        if (choice < 1 || choice > n)
        {
            printf("INVALID CHOICE\n");
            continue;
        }

        printf("ENTER QUANTITY: ");
        scanf("%d", &qty);

        if (qty < 1)
        {
            printf("INVALID QUANTITY\n");
            continue;
        }

        c.qty[choice - 1] += qty;
        c.total += price[choice - 1] * qty;
    }

    return c;
}

void printBill(char restname[], struct Customer c, int n, char dish[][50], float price[])
{
    int i;

    printf("\n==============================\n");
    printf("RESTAURANT: %s\n", restname);
    printf("CUSTOMER: %s\n", c.name);
    printf("ITEM        QTY     AMOUNT\n");

    for (i = 0; i < n; i++)
    {
        if (c.qty[i] > 0)
        {
            printf("%-10s %-6d %.2f\n",
                   dish[i],
                   c.qty[i],
                   c.qty[i] * price[i]);
        }
    }

    printf("------------------------------\n");
    printf("TOTAL BILL: %.2f\n", c.total);
    printf("==============================\n");
}
int main()
{
    int n, customers, i;
    char restname[50];

    printf("ENTER RESTAURANT NAME: ");
    scanf("%s",restname);

    printf("ENTER NUMBER OF DISHES: ");
    scanf("%d", &n);

    char dish[n][50];
    float price[n];

    inputMenu(n, dish, price);

    printf("ENTER NUMBER OF CUSTOMERS: ");
    scanf("%d", &customers);

    struct Customer c[customers];

    for (i = 0; i < customers; i++)
    {
        printf("\nENTER CUSTOMER %d NAME: ", i + 1);
        scanf("%s", c[i].name);

        c[i] = takeOrder(c[i], n, price);
        printBill(restname, c[i], n, dish, price);
    }

    printf("\nTHANK YOU FOR VISITING %s", restname);

    return 0;
}
