## Challegen


Source Code

#include <iostream>

using namespace std;

int check_flag(int data[])

{

    int a[] = {
    
        73,167,324,545,1360,2265,7908,133,160,277,860,1705,3696,
        
        6249,303,433,452,1097,2084,3401,6672,538,704,909,1520,2497,
        
        2244,6809,833,941,1096,1353,1824,2625,4228,1276,1546};
    
    for (int i = 0; i < 37; i++)
    
    {
    
        if (data[i] != a[i])
        
            return 0;
    
    }
    
    return 1;

}

int s_203_key(string data)

{

    int src[100];
    
    for (int i = 0; i < data.length(); i++)
    
    {
    
        src[i] = (((int)data[i] << ((char)i % 7)) + i * i);
    
    }
    
    check_flag(src);

}

int main()

{

    string flag;
    
    cout << "_______________BAT_DAU_______________" << endl;
    
    cout << "Inputflag : ";
    
    cin >> flag;
    
    if (s_203_key(flag) == 1)
    
        cout << "correct!!!";
    
    else
    
        cout << "incorrect!!!";

}

## Solve

![image](https://github.com/user-attachments/assets/1fc359ce-a018-47b6-8fca-85c308eaa26b)

![image](https://github.com/user-attachments/assets/185172b5-c0a1-4ece-865b-79b6c2fad7f7)

- Flag

`
ISPCTF{T01_co_kh1en_ban_vu1_12112003}
`

