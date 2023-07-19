     //Grocery store-
  //It's my class work project.
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

struct grocery {
    char gro_name[50];
    char gro_category[50];
    char gro_expiryDate[50];
    int gro_price;
    int gro_stock;
    int gro_code;
} g;

struct buyer {
    char buyer_name[50];
    char buyer_cnic[20];
    int gro_code;
} b;

FILE* fp;

void add_grocery() {
    fp = fopen("grocery.txt", "a");

    printf("Enter Grocery Name: ");
    getchar();
    gets(g.gro_name);

    printf("Enter Grocery Category: ");
    gets(g.gro_category);

    printf("Enter Grocery Expire Date: ");
    gets(g.gro_expiryDate);

    printf("Enter Grocery Code : ");
    scanf("%d", &g.gro_code);

    printf("Enter Grocery Price : ");
    scanf("%d", &g.gro_price);

    printf("Enter Grocery Stock: ");
    scanf("%d", &g.gro_stock);

    fwrite(&g, sizeof(g), 1, fp);
    fclose(fp);

    printf("\nGrocery Added Successfully!!\n");
}

void grocery_list() {
    printf("\n<== Available Grocery ==>\n");

    fp = fopen("grocery.txt", "r");
    while (fread(&g, sizeof(g), 1, fp) == 1) {
        printf("\nGrocery Name: %s\nGrocery Category: %s\nExpire Date: %s\nGrocery Code: %d\nGrocery Price: %d\nAvailable Stock: %d\n",
            g.gro_name, g.gro_category, g.gro_expiryDate, g.gro_code, g.gro_price, g.gro_stock);
    }

    fclose(fp);
}

void del_grocery() {
    FILE* ft;

    int found = 0;
    int code;

    printf("Enter Grocery Code: ");
    scanf("%d", &code);

    fp = fopen("grocery.txt", "r");
    ft = fopen("temp.txt", "w");

    while (fread(&g, sizeof(g), 1, fp) == 1) {
        if (code == g.gro_code) {
            found = 1;
        } else {
            fwrite(&g, sizeof(g), 1, ft);
        }
    }

    if (found == 1) {
        printf("\nGrocery is Deleted Successfully!!\n");
    } else {
        printf("\nRecord is Not Found!!\n");
    }

    fclose(fp);
    fclose(ft);

    remove("grocery.txt");
    rename("temp.txt", "grocery.txt");
}

void buy_grocery() {
    int found = 0;

    printf("Enter Grocery Code: ");
    scanf("%d", &b.gro_code);

    fp = fopen("grocery.txt", "r");

    while (fread(&g, sizeof(g), 1, fp) == 1) {
        if (g.gro_code == b.gro_code) {
            b.gro_code = g.gro_code;
            found = 1;
            break;
        }
    }

    fclose(fp);

    if (found == 0) {
        printf("\nGrocery Not Found!!\n");
        printf("Please Try Again!!\n");
        return;
    }

    fp = fopen("buy.txt", "a");

    printf("Enter Buyer Name: ");
    getchar();
    gets(b.buyer_name);

    printf("Enter Buyer CNIC: ");
    gets(b.buyer_cnic);

    printf("\nGrocery Bought Successfully!!\n");

    fwrite(&b, sizeof(b), 1, fp);
    fclose(fp);
}

int main() {
    int operation;

    printf("\n---Welcome to the Grocery Store---\n");

    while (1) {
        printf("\n<== Available Options: ==>\n");
        printf("\n1. Available Grocery\n");
        printf("2. Add Grocery\n");
        printf("3. Delete Grocery\n");
        printf("4. Buy Grocery\n");
        printf("0. Exit\n\n");

        printf("Enter Your Choice: ");
        scanf("%d", &operation);
        getchar();

        switch (operation) {
            case 0:
                printf("\nThank You\nGoodbye\n");
                exit(0);
            case 1:
                grocery_list();
                break;
            case 2:
                add_grocery();
                break;
            case 3:
                del_grocery();
                break;
            case 4:
                buy_grocery();
                break;
            default:
                printf("Invalid Choice!!\n\n");
        }
    }

    return 0;
}
