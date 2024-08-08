## Challegen


Source Code

#include <stdio.h>

#include <string.h>

char LAW(char a, char b) {

    return !(a | b); 

}

char LOST(char a) {

    return LAW(a, a); // luôn in ra 1

}

char END(char a, char b) {

    return LOST(LAW(a, b));

}

char OO(char a, char b) {

    return LOST(END(LOST(a), LOST(b)));

}

char ISPCLUB(char a, char b) {

    return END(OO(a, LOST(b)), OO(LOST(a), b));

}

int main() {

    unsigned char input[15];
    
    unsigned char cipher[15] = { 0xe7,0x99,0xdb,0xf6,0x98,0xda,0xf6,0xda,0x99,0xf6,0xe4,0x9d,0xce,0x98,0xca };
    
    unsigned char your_cipher[15];
    
    char key[] = { 1,0,0,1,0,1,0,1 };
    
    printf("Enter Flag : ");

    fgets((char*)input, sizeof(input) + 1, stdin);

    for (char i = 0; i < sizeof(input); i++) {

        char tmp[8] = { 0,0,0,0,0,0,0,0 };
        
        unsigned char result = 0;

        for (char b = 0; b < 8; b++) {
        
            char bit_1 = (input[i] >> b) & 1;
            
            char bit_2 = key[b];
            
            char rs = ISPCLUB(bit_1, bit_2);
            
            tmp[b] = rs;
        
        }
        
        for (char k = 7; k >= 0; k--) {
        
            result = (result << 1) + tmp[k];
        
        }
        
        your_cipher[i] = result;
    
    }
    
    for (char i = 0; i < 15; i++) {
    
        if (your_cipher[i] != cipher[i]) {
        
            printf("Incorrect!");
            
            return 1;
        
        }
    
    }
    
    printf("GOOD! HERE IS YOUR FLAG ISPCTF{%s}", input);

}

## Solve

//0xa9 là dạng hex của dãy key[]={1,0,0,1,0,1,0,1} mình sẽ đảo ngược chuỗi key ta sẽ đc dạng hex 0xa9
        
    }
}
//flag=ISPCTF{N0r_1s_s0_M4g1c}
