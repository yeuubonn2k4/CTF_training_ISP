# ĐỀ BÀI 

int fuzz(char *key) {
    char char1[10], char2[10], char3[10], char4[10];
    memset(char1, 0, 10);
    memset(char2, 0, 10);
    memset(char3, 0, 10);
    memset(char4, 0, 10);

    strncpy(char1, key, 8);
    strncpy(char2, key + 8, 8);
    strncpy(char3, key + 16, 8);
    strncpy(char4, key + 24, 8);

    memset(key, 0, 32);

    strcat(key, char3);
    strcat(key, char1);
    strcat(key, char4);
    strcat(key, char2);

    return 1;
}
int main(int argc, char **argv) {
    printf("Let's see if you passed the right flag...\n");
    if (argc == 2)
        if (strlen(*(argv + 1)) == 32)
            if (!fuzz(*(argv + 1)))
                printf("Wrong Direction.");
            else if (!strncmp(*(argv + 1), "302753d5b52596eb75b8", 0x14))
                if (!strncmp(*(argv + 1) + 20, "9c11cc30e5c7", 12))
                    printf("True.");
                else
                    printf("Try again.");
            else
                printf("Try again.");
        else
            printf("Try again.");
    else
        printf("Try again.");
}
// Hàm memset : Đặt một giá trị cụ thể vào một phần hoặc toàn bộ của mảng dữ liệu

> void *memset(void *ptr, int value, size_t num);

// Hàm strncpy : sao chép A sang B 

> char *strncpy(char *destination, const char *source, size_t num);

// Hàm strcat : Nối chuỗi nguồn vào cuối của chuỗi đích và trả về con trỏ tới chuỗi đích

gộp 302753d5b52596eb75b89c11cc30e5c7

-> Flag: ISP{302753d5b52596eb75b89c11cc30e5c7}




