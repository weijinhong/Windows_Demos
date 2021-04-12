*** 
### **目录**
* [Windows编程基础](Windows编程基础)
* [Duilib使用](Duilib使用)
***

#### Windows编程基础
##### 一、入口函数
1.ASCII编码控制台程序主函数  
int main(int argc, char **argv)  

2.UNICODE编码控制台程序主函数  
int wmain(int argc, wchar_t **argv)  

3.通过宏_UNICODE决定是上面哪个函数  
int _tmain(int argc, TCHAR **argv)  

4.ASCII编码Windows桌面程序主函数  
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR pCmdLine, int nCmdShow)  

5.UNICODE编码Windows桌面程序主函数  
int WINAPI wWinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR pCmdLine, int nCmdShow)  

6.通过宏_UNICODE决定是上面那个函数  
int WINAPI _tWinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR pCmdLine, int nCmdShow)  

ASCII编码：用1个字节表示一个字符，只能表示英文字符；  
UNICODE编码：通常用2个字节表示一个字符，偏僻字符用4个字节表示，能表示所有语言的字符；  
UTF-8编码：可变长编码，根据大小用1-6个字节表示一个字符，能表示所有语言的字符；  
WinMain函数参数：第一个参数在早期的Windows版本中使用，是程序实体的句柄，第二个参数在早期的Windows版本中使用，新版本始终为NULL(定义为0)，第三个参数是执行程序的命令列，是UNICODE字符串，第四个参数是程序最初显示方式(最小化最大化)；  

##### 二、WinMain函数的实现
1.创建窗口类  

    const wchar_t CLASS_NAME[] = L"Sample Window Class";  
    WNDCLASS wc = { };  
    wc.lpfnWndProc = WindowProc;  // 窗口过程函数指针  
    wc.hInstance = hInstance;  // 应用程序实例句柄  
    wc.lpszClassName = CLASS_NAME;  // 窗口类标识  

2.注册窗口类  

    RegisterClass(&wc);  

3.创建窗口

    HWND hwnd = CreateWindowEx(
    0,                              // Optional window styles.
    CLASS_NAME,                     // Window class
    L"Learn to Program Windows",    // Window text
    WS_OVERLAPPEDWINDOW,            // Window style
    // Size and position
    CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
    NULL,       // Parent window    
    NULL,       // Menu
    hInstance,  // Instance handle
    NULL        // Additional application data
    );
    if (hwnd == NULL)
    {
        return 0;
    }

4.显示窗口  

    ShowWindow(hwnd, nCmdShow);

6.消息侦听  

    MSG msg = { };
    while (GetMessage(&msg, NULL, 0, 0) > 0)
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

##### 三、窗口过程函数WindowProc

    LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam)
    {
        switch (uMsg)
        {
        case WM_SIZE:
            {
                int width = LOWORD(lParam);  // Macro to get the low-order word.
                int height = HIWORD(lParam); // Macro to get the high-order word.
    
                // Respond to the message:
                OnSize(hwnd, (UINT)wParam, width, height);
            }
            break;
        }
    }
    
    void OnSize(HWND hwnd, UINT flag, int width, int height)
    {
        // Handle resizing
    }


#### Duilib使用
##### 一、源码地址

https://github.com/duilib/duilib

https://github.com/qdtroy/DuiLib_Ultimate
