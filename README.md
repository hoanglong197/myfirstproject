#include <stdio.h>
#include <stdlib.h>

struct SinhVien {
    char mssv[20];
    char hoten[50];
    float diemTB;
};

int isPrime(int num) {
    if (num < 2) {
        return 0;
    }
    for (int i = 2; i * i <= num; ++i) {
        if (num % i == 0) {
            return 0;
        }
    }
    return 1;
}

void recursiveArray(int arr[], int n, int index, int *sumPrime) {
    if (index < n) {
        printf("Nhap phan tu thu %d: ", index + 1);
        scanf("%d", &arr[index]);

        if (isPrime(arr[index])) {
            *sumPrime += arr[index];
        }

        recursiveArray(arr, n, index + 1, sumPrime);
    }
}

void inputStudentsAndSort() {
    int n;
    printf("Nhap so luong sinh vien: ");
    scanf("%d", &n);

    struct SinhVien *dsSinhVien = (struct SinhVien *)malloc(n * sizeof(struct SinhVien));

    for (int i = 0; i < n; ++i) {
        printf("Nhap thong tin sinh vien thu %d:\n", i + 1);
        printf("MSSV: ");
        scanf("%s", dsSinhVien[i].mssv);
        printf("Ho ten: ");
        getchar(); 
        gets(dsSinhVien[i].hoten);
        printf("Diem TB: ");
        scanf("%f", &dsSinhVien[i].diemTB);
    }

    FILE *file = fopen("D:\\dssinhvien.txt", "w");
    if (file == NULL) {
        printf("Khong the mo file.\n");
        return;
    }

    fprintf(file, "%d\n", n);
    for (int i = 0; i < n; ++i) {
        fprintf(file, "%s %s %.2f\n", dsSinhVien[i].mssv, dsSinhVien[i].hoten, dsSinhVien[i].diemTB);
    }

    fclose(file);

    for (int i = 0; i < n - 1; ++i) {
        for (int j = i + 1; j < n; ++j) {
            if (dsSinhVien[i].diemTB > dsSinhVien[j].diemTB) {
                struct SinhVien temp = dsSinhVien[i];
                dsSinhVien[i] = dsSinhVien[j];
                dsSinhVien[j] = temp;
            }
        }
    }

    printf("\nDanh sach sinh vien theo thu tu tang dan cua DiemTB:\n");
    for (int i = 0; i < n; ++i) {
        printf("%s %s %.2f\n", dsSinhVien[i].mssv, dsSinhVien[i].hoten, dsSinhVien[i].diemTB);
    }

    free(dsSinhVien);
}

int main() {
    int choice;
    do {
        printf("===== MENU =====\n");
        printf("1. Su dung de quy de nhap-xuat mang va tinh tong so nguyen to\n");
        printf("2. Nhap danh sach sinh vien va sap xep theo DiemTB\n");
        printf("0. Thoat\n");
        printf("Nhap lua chon: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: {
                int n;
                printf("Nhap so phan tu cua mang: ");
                scanf("%d", &n);
                int arr[n];
                int sumPrime = 0;

                recursiveArray(arr, n, 0, &sumPrime);

                printf("\nMang vua nhap: ");
                for (int i = 0; i < n; ++i) {
                    printf("%d ", arr[i]);
                }

                printf("\nTong cac so nguyen to trong mang: %d\n", sumPrime);
                break;
            }
            case 2: {
                inputStudentsAndSort();
                break;
            }
            case 0: {
                printf("Thoat chuong trinh.\n");
                break;
            }
            default:
                printf("Lua chon khong hop le. Vui long chon lai.\n");
        }

    } while (choice != 0);

    return 0;
}
