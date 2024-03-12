# Lab-1-SDA

prob 1

#include <stdio.h>

struct vitamins_list_in_clinic {
    int id;
    float price_usd;
    char vitamin_type;
    double mg_of_specific_vitamin;
};
#define array_size 100
union search_value {
    int id;
    float price_usd;
    char vitamin_type;
    double mg_of_specific_vitamin;
};
void read_elements(struct vitamins_list_in_clinic array[], int size) {
    printf("Enter the elements of the array:\n");
    for (int i = 0; i < size; i++) {
        printf("Element %d:\n", i + 1);
        printf("  Enter id value: ");
        scanf("%d", &array[i].id);
        printf("  Enter price (USD): ");
        scanf("%f", &array[i].price_usd);
        printf("  Enter vitamin type: ");
        scanf(" %c", &array[i].vitamin_type);
        printf("  Enter mg of specific vitamin: ");
        scanf("%lf", &array[i].mg_of_specific_vitamin);
    }
}
void display_elements(struct vitamins_list_in_clinic array[], int size) {
    printf("The elements of the array are:\n");
    for (int i = 0; i < size; i++) {
        printf("Element %d:\n", i + 1);
        printf("  Id value: %d\n", array[i].id);
        printf("  Price (USD): %.2f\n", array[i].price_usd);
        printf("  Vitamin type: %c\n", array[i].vitamin_type);
        printf("  Mg of specific vitamin: %.4lf\n", array[i].mg_of_specific_vitamin);
    }
}
int search_element(struct vitamins_list_in_clinic array[], int size, union search_value value_to_find, int type) {
    for (int i = 0; i < size; i++) {
        switch(type) {
            case 1:
                if (array[i].id == value_to_find.id) {
                    return i;
                }
                break;
            case 2:
                if (array[i].price_usd == value_to_find.price_usd) {
                    return i;
                }
                break;
            case 3:
                if (array[i].vitamin_type == value_to_find.vitamin_type) {
                    return i;
                }
                break;
            case 4:
                if (array[i].mg_of_specific_vitamin == value_to_find.mg_of_specific_vitamin) {
                    return i;
                }
                break;
            default:
                return -1;
        }
    }
    return -1;
}

int main() {
    struct vitamins_list_in_clinic array[array_size];
    int size;
    printf("Enter the size of the array (max %d): ", array_size);
    scanf("%d", &size);
    if (size > array_size || size <= 0) {
        printf("Invalid size!\n");
        return 1;
    }
    read_elements(array, size);
    display_elements(array, size);
    union search_value value_to_search;
    int search_type;
    printf("Enter the type of value to search for (1: id, 2: price (USD), 3: vitamin type, 4: mg of specific vitamin): ");
    scanf("%d", &search_type);
    switch(search_type) {
        case 1:
            printf("Enter the id value you want to search for: ");
            scanf("%d", &value_to_search.id);
            break;
        case 2:
            printf("Enter the price (USD) value you want to search for: ");
            scanf("%f", &value_to_search.price_usd);
            break;
        case 3:
            printf("Enter the vitamin type you want to search for: ");
            scanf(" %c", &value_to_search.vitamin_type);
            break;
        case 4:
            printf("Enter the mg of specific vitamin value you want to search for: ");
            scanf("%lf", &value_to_search.mg_of_specific_vitamin);
            break;
        default:
            printf("Invalid search type!\n");
            return 1;
    }
    int position = search_element(array, size, value_to_search, search_type);
    if (position != -1) {
        printf("The element was found at position %d.\n", position);
    } else {
        printf("The element was not found.\n");
    }
    return 0;
}

prob 2

#include <stdio.h>
#include <stdlib.h>

typedef struct vitamins_list_in_clinic{
    int id;
    float price_usd;
    char vitamin_type;
    double mg_of_specific_vitamin;
} element;

void read_elements(element *array, int size);
void display_elements(element *array, int size);
int search_element(element *array, int size, int field, void *value);
void release_memory(element *array);
void sort_elements(element *array, int size);
void insert_element_at_end(element **array, int *size, element new_element);
void insert_element_at_beginning(element **array, int *size, element new_element);
void insert_element_at_position(element **array, int *size, int position, element new_element);
void delete_element_at_position(element **array, int *size, int position);

int main() {
    int size;
    printf("Enter the size of the array: ");
    scanf("%d", &size);
    element *array = (element *)malloc(size * sizeof(element));
    if (array == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }
    read_elements(array, size);
    int choice;
    do {
        printf("\nMenu:\n");
        printf("1. Display elements\n");
        printf("2. Search element\n");
        printf("3. Sort elements\n");
        printf("4. Insert element at end\n");
        printf("5. Insert element at beginning\n");
        printf("6. Insert element at position\n");
        printf("7. Delete element at position\n");
        printf("8. Release memory\n");
        printf("9. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                display_elements(array, size);
                break;
            case 2: {
                printf("Select field to search by:\n");
                printf("1. ID\n");
                printf("2. Price (USD)\n");
                printf("3. Vitamin Type\n");
                printf("4. Milligrams of Specific Vitamin\n");
                int field_choice;
                scanf("%d", &field_choice);
                void *value;
                switch (field_choice) {
                    case 1:
                        printf("Enter the value to search (ID): ");
                        value = malloc(sizeof(int));
                        scanf("%d", (int *)value);
                        break;
                    case 2:
                        printf("Enter the value to search (Price in USD): ");
                        value = malloc(sizeof(float));
                        scanf("%f", (float *)value);
                        break;
                    case 3:
                        printf("Enter the value to search (Vitamin Type): ");
                        value = malloc(sizeof(char));
                        scanf(" %c", (char *)value);
                        break;
                    case 4:
                        printf("Enter the value to search (Milligrams of Specific Vitamin): ");
                        value = malloc(sizeof(double));
                        scanf("%lf", (double *)value);
                        break;
                    default:
                        printf("Invalid choice\n");
                        continue;
                }
                int position = search_element(array, size, field_choice, value);
                if (position != -1)
                    printf("Element found at position %d\n", position);
                else
                    printf("Element not found\n");
                free(value);
                break;
            }
            case 3:
                sort_elements(array, size);
                printf("Elements sorted\n");
                break;
            case 4: {
                element new_element;
                printf("Enter the id of the new element: ");
                scanf("%d", &new_element.id);
                printf("Enter the price in USD of the new element: ");
                scanf("%f", &new_element.price_usd);
                printf("Enter the vitamin type of the new element: ");
                scanf(" %c", &new_element.vitamin_type);
                printf("Enter the milligrams of specific vitamin of the new element: ");
                scanf("%lf", &new_element.mg_of_specific_vitamin);
                insert_element_at_end(&array, &size, new_element);
                break;
            }
            case 5: {
                element new_element;
                printf("Enter the id of the new element: ");
                scanf("%d", &new_element.id);
                printf("Enter the price in USD of the new element: ");
                scanf("%f", &new_element.price_usd);
                printf("Enter the vitamin type of the new element: ");
                scanf(" %c", &new_element.vitamin_type);
                printf("Enter the milligrams of specific vitamin of the new element: ");
                scanf("%lf", &new_element.mg_of_specific_vitamin);
                insert_element_at_beginning(&array, &size, new_element);
                break;
            }
            case 6: {
                int position;
                printf("Enter the position to insert: ");
                scanf("%d", &position);
                element new_element;
                printf("Enter the id of the new element: ");
                scanf("%d", &new_element.id);
                printf("Enter the price in USD of the new element: ");
                scanf("%f", &new_element.price_usd);
                printf("Enter the vitamin type of the new element: ");
                scanf(" %c", &new_element.vitamin_type);
                printf("Enter the milligrams of specific vitamin of the new element: ");
                scanf("%lf", &new_element.mg_of_specific_vitamin);
                insert_element_at_position(&array, &size, position, new_element);
                break;
            }
            case 7: {
                int position;
                printf("Enter the position to delete: ");
                scanf("%d", &position);
                delete_element_at_position(&array, &size, position);
                break;
            }
            case 8:
                release_memory(array);
                printf("Memory released\n");
                break;
            case 9:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice\n");
        }
    } while (choice != 9);

    return 0;
}

void read_elements(element *array, int size) {
    printf("Enter %d elements:\n", size);
    for (int i = 0; i < size; ++i) {
        printf("Enter id for element %d: ", i + 1);
        scanf("%d", &array[i].id);
        printf("Enter price in USD for element %d: ", i + 1);
        scanf("%f", &array[i].price_usd);
        printf("Enter vitamin type for element %d: ", i + 1);
        scanf(" %c", &array[i].vitamin_type);
        printf("Enter milligrams of specific vitamin for element %d: ", i + 1);
        scanf("%lf", &array[i].mg_of_specific_vitamin);
    }
}

void display_elements(element *array, int size) {
    printf("Elements:\n");
    for (int i = 0; i < size; ++i) {
        printf("ID: %d, Price (USD): %.2f, Vitamin Type: %c, Milligrams of Specific Vitamin: %.4lf\n",
               array[i].id, array[i].price_usd, array[i].vitamin_type, array[i].mg_of_specific_vitamin);
    }
}

int search_element(element *array, int size, int field, void *value) {
    for (int i = 0; i < size; ++i) {
        switch (field) {
            case 1:
                if (array[i].id == *((int *)value)) {
                    return i;
                }
                break;
            case 2:
                if (array[i].price_usd == *((float *)value)) {
                    return i;
                }
                break;
            case 3:
                if (array[i].vitamin_type == *((char *)value)) {
                    return i;
                }
                break;
            case 4:
                if (array[i].mg_of_specific_vitamin == *((double *)value)) {
                    return i;
                }
                break;
        }
    }
    return -1;
}

void release_memory(element *array) {
    free(array);
}

void sort_elements(element *array, int size) {
}

void insert_element_at_end(element **array, int *size, element new_element) {
    *size += 1;
    *array = realloc(*array, (*size) * sizeof(element));
    (*array)[*size - 1] = new_element;
}

void insert_element_at_beginning(element **array, int *size, element new_element) {
    *size += 1;
    *array = realloc(*array, (*size) * sizeof(element));
    for (int i = *size - 1; i > 0; --i) {
        (*array)[i] = (*array)[i - 1];
    }
    (*array)[0] = new_element;
}

void insert_element_at_position(element **array, int *size, int position, element new_element) {
    if (position < 0 || position > *size) {
        printf("Invalid position\n");
        return;
    }
    *size += 1;
    *array = realloc(*array, (*size) * sizeof(element));
    for (int i = *size - 1; i > position; --i) {
        (*array)[i] = (*array)[i - 1];
    }
    (*array)[position] = new_element;
}

void delete_element_at_position(element **array, int *size, int position) {
    if (position < 0 || position >= *size) {
        printf("Invalid position\n");
        return;
    }
    for (int i = position; i < *size - 1; ++i) {
        (*array)[i] = (*array)[i + 1];
    }
    *size -= 1;
    *array = realloc(*array, (*size) * sizeof(element));
}
