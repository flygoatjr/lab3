#define _CRT_SECURE_NO_WARNINGS 
#include <stdio.h> 
#include <stdlib.h> 
#include <string.h> 
#include <locale.h> 
#include<Windows.h>

#define MAX_LENGTH 1000 // Максимальная длина строки 
#define MAX_WORDS 100 // Максимальное количество слов в строке

void getInput(char* str) { // Функция для ввода текста 
    fgets(str, MAX_LENGTH, stdin); }

    int split_into_words(char* text, char words[][MAX_LENGTH]) { // Функция для разделения текста на слова 
        int count = 0; 
        char* token = strtok(text, " \t\n"); 
        while (token != NULL && count < MAX_WORDS) { 
            strcpy(words[count++], token); 
            token = strtok(NULL, " \t\n"); } return count; // Возвращаем количество найденных слов 
    }

        void form_width(char* text, int width) { // Форматирование текста по ширине 
            char words[MAX_WORDS][MAX_LENGTH]; 
            int wordCount = split_into_words(text, words); 
            int currentLength = 0;

            for (int i = 0; i < wordCount; i++) {
                if (currentLength + strlen(words[i]) >= width) {
                    printf("\n");                   // Перенос строки
                    currentLength = 0;
                }

                if (currentLength > 0) {          // Добавляем пробел только если не первое слово в строке
                    printf(" ");
                    currentLength++;
                }

                printf("%s", words[i]);
                currentLength += strlen(words[i]);
            }
            printf("\n");
        }

        // Форматирование текста по центру 
            void form_center(char* text, int width) { 
                char words[MAX_WORDS][MAX_LENGTH]; 
                int wordCount = split_into_words(text, words); 
                int totalLength = 0;

        for (int i = 0; i < wordCount; i++) {
            for (int j = 0; j < wordCount; j++) {
                totalLength += strlen(words[j]);
                if ((totalLength + strlen(words[j + 1])) > width)
                    printf("\n");
                break;
            }
        }

        int spacesNeeded = (width - totalLength) / 2;

        for (int i = 0; i < spacesNeeded; i++) {      // Выводим пробелы для выравнивания
            printf(" ");
        }

        for (int i = 0; i < wordCount; i++) {         // Добавляем пробел между словами
            printf("%s", words[i]);
            if (i < wordCount - 1) {
                printf(" ");
            }
        }
        printf("\n");
    }

    // Форматирование текста по левому краю 
        void form_left(char* text) { 
            printf("%s\n", text); // Выводим текст без изменений 
        }

    // Форматирование текста по правому краю 
    void form_right(char* text, int width) { 
        char words[MAX_WORDS][MAX_LENGTH]; 
        int wordCount = split_into_words(text, words);

    int totalLength = strlen(text);
    int spaces = (totalLength < width) ? (width - totalLength) : 0;

    for (int i = 0; i < spaces; i++) {
        printf(" ");  // Выводим пробелы для выравнивания
    }

    printf("%s\n", text);
}

int main() { // Взаимодействие с пользователем 
    char text[MAX_LENGTH]; 
    int choice; 
    int width;

    setlocale(LC_ALL, "RUS");
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);
    

    do {
        printf("Введите текст : \n");
        getInput(text);
        printf("\n Исходный текст : %s", text);

        printf("\n Выберите способ форматирования : \n");
        printf("1. Форматировать по ширине \n");
        printf("2. Форматировать по центру \n");
        printf("3. Форматировать по левому краю \n");
        printf("4. Форматировать по правому краю \n");
        printf("5. Выйти \n");

        printf("Ваш выбор : ");
        scanf("%d", &choice); 
        if (choice == 1 || choice == 2 || choice == 4) {
            printf("Введите ширину окна : ");
            scanf("%d", &width);
        }

        switch (choice) {
        case 1:
            printf("\n Форматированный текст (по ширине) : \n");
            form_width(text, width);
            break;
        case 2:
            printf("\n Форматированный текст (по центру) : \n");
            form_center(text, width);
            break;
        case 3:
            printf("\n Форматированный текст (по левому краю) : \n");
            form_left(text);
            break;
        case 4:
            printf("\n Форматированный текст (по правому краю) : \n");
            form_right(text, width);
            break;
        case 5:
            printf("Завершение работы. \n");
            return 0;
        default:
            printf("Неверный выбор. Попробуйте снова. \n");
        }
        printf("\n");
        getchar();      // Очистка буфера

    } while (true);
}
