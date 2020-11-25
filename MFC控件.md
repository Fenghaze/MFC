# 使用VS2019向导创建MFC项目

- 安装MFC组件
- 创建项目选择MFC应用，选择**基于对话框**

- 编写MFC项目时，打开【视图-类视图】进行编辑

![](D:/mynotes/MFC/assets/类视图.png)

CDialogApp：是应用程序类的实现

CDialogDlg：是主窗口的实现

- 打开【视图-资源视图】，使用UI界面**绘制**对话框，右键【属性】进行编辑
  - 每个对话框都有一个ID

![](D:/mynotes/MFC/assets/资源视图.png)

- 新的对话框绘制成功后，右键【生成类】，为这个对话框生成源文件，在**类视图**中会显示新添加的这个对话框类

![](D:/mynotes/MFC/assets/添加MFC类.png)

![](D:/mynotes/MFC/assets/类视图2.png)

- 设置按钮的响应事件：
  - 方法一：右键按钮【属性】，设置控件事件
  - 方法二：右键按钮【添加事件处理程序】
  - ==方法三：双击按钮，自动生成响应函数==

# CString 和 string 

```c++
CString cstr;
string str;

//CString转为string
str = CT2A(cstr.GetString());

//stirng转为CString
cstr = str.c_str();

// string转为char(数据库的字符串是char类型)
char buf[255];
buf = str.c_str();
```



# 模态框、非模态框

- 鼠标响应事件：弹出模态框（阻塞的）

```c++
//模态对话框点击事件
void CDialogDlg::OnBnClickedButton1()
{
	// TODO: 在此添加控件通知处理程序代码
	//弹出模态对话框
	Dialog1 dlg1;
	dlg1.DoModal();	
}
```

- 鼠标响应事件：弹出非模态框

  ==注意：==

  - 1、将非模态对话框类设置为**父窗口**`DialogDlg.h`的成员变量
  - 2、在`CDialogDlg::OnInitDialog()`函数中进行初始化，只能==创建一次==窗口
  - 3、在响应事件中显示对话框

```c++
dlg2.Create(IDD_DIALOG2);	//创建对话框，只能创建一次，多次创建会崩溃
dlg2.ShowWindow(SW_SHOWNORMAL);	//显示对话框
```



# 静态文本

- 在【工具箱】中添加【Static Text】控件
- 如果要获取静态文本的内容，需要右键【属性】，以==控件（control）的方式==添加变量，设置变量名`m_text`
  - 静态文本控件不支持添加变量，需要在之前修改静态文本控件的ID
- 添加成功后，可以在【类视图】中看到新增的变量名

- 设置文本内容：

```c++
m_text.SetWindowTextW(TEXT("呵呵"));	//设置文本内容
```

- 获取文本内容：

```c++
CString str;
m_text.GetWindowTextW(str);	//获取文本内容
MessageBox(str);	//弹出
```

- 以==值（value）的方式==添加变量

  - 设置文本内容：

    ```C++
    //将value同步到控件中
    m_text = TEXT("hello");
    UpdateData(FALSE);
    ```

  - 获取文本内容：

    ```c++
    //将控件内容同步到value中
    UpdateData(TRUE);
    MessageBox(m_text);
    ```

- 使用静态文本设置一张图片（.bmp格式）

```c++
//设置静态控件窗口风格为位图居中显示
m_pic.ModifyStyle(0xf, SS_BITMAP | SS_CENTERIMAGE);

//通过路径获取bitmap句柄
#define HBMP(filepath,width,height) (HBITMAP)LoadImage(AfxGetInstanceHandle(),filepath,IMAGE_BITMAP,width,height,LR_LOADFROMFILE|LR_CREATEDIBSECTION)

//图片大小应该按照控件大小设置
CRect rect;
m_pic.GetWindowRect(rect);

//静态控件设置bitmap
m_pic.SetBitmap(HBMP(TEXT("./24.bmp"), rect.Width(), rect.Height()));
```

# 按钮

- 点击事件：双击按钮，生成点击事件函数
- 获取按钮的值，需要右键【属性】，添加变量，设置变量名

- 设置和获取内容的方法与静态文本一样
- 设置点击状态：
  - 禁用按钮：`m_btn.EnableWindow(FALSE);`




## 退出当前窗口

```c++
CDialog::OnOK();		//方法一
CDialog::OnCancel();	//方法二

exit(0);	//关闭程序

//退出程序
AfxGetMainWnd()->SendMessage(WM_CLOSE);

//关闭当前窗口
DestroyWindow( );

//关闭模式对话框
EndDialog(0);
```



# 编辑框

- 在【工具箱】中添加【Edit Control】控件

- 常用属性

|     **属性**      |                           **含义**                           |
| :---------------: | :----------------------------------------------------------: |
|      Number       |                       True只能输入数字                       |
|     Password      |                         True密码模式                         |
|    Want return    |     True接收回车键，自动换行，只有在多行模式下，才能换行     |
|     Multiline     |                         True多行模式                         |
|   Auto VScroll    | True 当垂直方向字符太多，自动出现滚动条，同时设置Vertical Scroll才有效 |
|  Vertical Scroll  | True当垂直方向字符太多，自动出现滚动条，和Auto  VScroll配合使用 |
| Horizontal Scroll | True当垂直方向字符太多，自动出现滚动条，和Auto  HScroll配合使用 |
|     Read Only     |                          True 只读                           |

- 获取按钮的值，需要右键【属性】，添加变量，设置变量名
- 设置和获取内容的方法与静态文本一样



- 默认的编辑框，按【Enter】键会自动关闭对话框，如果要更改这个设置需要进行如下操作：
  - 在【类视图】中右键对话框类【属性】，找到【重写】
  - 设置【OnOK】事件，注释掉`CDialogEx::OnOK()`



# 下拉框

- 在【工具箱】中添加【Combo Box】控件
- 设置变量名
- 在初始化函数`CComboBoxDlg::OnInitDialog`中设定下拉选项
- 设置切换选项事件（选项发生改变时，执行某事件），右键【属性】->【控件事件】->【CBN_SELCHANGE】
- 常用API：

|          接口           |               描述               |
| :---------------------: | :------------------------------: |
|     AddString(TEXT)     |  添加下拉框选项，参数为选项内容  |
|     SetCurSel(int)      |     设置默认值，参数为索引值     |
|       GetCurSel()       |         获取当前选项索引         |
| InsertString(int, TEXT) | 插入选项，参数为索引值、选项内容 |
|    DeleteString(int)    |      删除选项，参数为索引值      |
|   GetLBText(int, str)   | 根据索引值获取内容，保存到str中  |



# 列表

- 在【工具箱】中添加【List Control】控件

- 常用属性设置：view -> Report(报表方式)

```c++
//列表
CString str[] = { TEXT("姓名"), TEXT("性别"), TEXT("年龄") };

for (size_t i = 0; i < 3; i++)
{
    //设置表头: 索引、内容、对齐方式、列宽
    m_list.InsertColumn(i, str[i], LVA_ALIGNLEFT, 100);
}
//设置正文
//插入第一列
m_list.InsertItem(0, TEXT("Mary"));
//给这个Item插入其他列的数据
m_list.SetItemText(0, 1, TEXT("女"));
m_list.SetItemText(0, 2, TEXT("32"));
```



# 树控件

- 【工具箱】添加【Tree Control】
- 常用属性：

|   **属性**    |    **含义**     |
| :-----------: | :-------------: |
|  has buttons  | True 有展开按钮 |
|   has lines   |  True 有展开线  |
| lines at root |  True 有根节点  |

- 对树控件进行初始化的步骤如下：
  - ==注==：将图片放置在项目的`res`文件夹中，打开【资源视图】->【Icon】->右键【添加资源】->【导入】

```c++
CImageList list;		//必须永久保存这个集合，需要定义在头文件中

//1、设置图标集合
//创建图片集合的空间(宽，高，图标格式，图标数量，图标数量)
list.Create(30, 30, ILC_COLOR32, 4, 4);
//准备HICON图标
HICON icons[4];
//从全局应用程序加载图标
icons[0] = AfxGetApp()->LoadIconW(IDI_ICON1);
icons[1] = AfxGetApp()->LoadIconW(IDI_ICON2);
icons[2] = AfxGetApp()->LoadIconW(IDI_ICON3);
icons[3] = AfxGetApp()->LoadIconW(IDI_ICON4);
//将图标添加到集合中
for (size_t i = 0; i < 4; i++)
{
    list.Add(icons[i]);
}
//将图标集合添加到树控件
m_tree.SetImageList(&list, TVSIL_NORMAL);

//2、设置节点
HTREEITEM root = m_tree.InsertItem(TEXT("根节点"), 0, 0, NULL);
HTREEITEM parent = m_tree.InsertItem(TEXT("父节点"), 1, 1, root);
HTREEITEM child1 = m_tree.InsertItem(TEXT("子节点1"), 2, 2, parent);
HTREEITEM child2 = m_tree.InsertItem(TEXT("子节点2"), 3, 3, parent);
```

- 设置选项切换事件：【属性】->【控件事件】->【SELCHANGED】、

```c++
//获取当前的选项
HTREEITEM item = m_tree.GetSelectedItem();
//获取当前项的名称
CString str = m_tree.GetItemText(item);
MessageBox(str);
```

- 常用API：

|            接口            |         功能         |
| :------------------------: | :------------------: |
|        AfxGetApp()         | 获取应用程序对象指针 |
|     CWinApp::LoadIcon      |    加载自定义图标    |
|     CImageList::Create     |     创建图像列表     |
|      CImageList::Add       |   图像列表追加图标   |
|  CTreeCtrl::SetImageList   |   设置图形状态列表   |
|   CTreeCtrl::InsertItem    |       插入节点       |
|   CTreeCtrl::SelectItem    |    设置默认选中项    |
| CTreeCtrl::GetSelectedItem |      获取选中项      |
|   CTreeCtrl::GetItemText   |     获取某项内容     |



# 标签页

- 【工具箱】添加【Tab Control】
- 复制两个文件`TabSheet.cpp`、`TabSheet.h`到项目根目录，【解决方案】->右键【属性】->添加【现有项】
- 【资源视图】->插入Dialog，设置Dialog属性【Border】为None，【Style】为Child

- 添加一个新的Dialog，需要右键【添加类】生成头文件

- 打开【资源视图】，为标签类添加变量



# 时间选择器

- 【工具箱】添加【Data Time Picker】
- 【属性】Format：短日期、时间
- 时间格式转换：

```c++
CDateTimeCtrl in_date;	//短日期
CTime in_date_t;
in_date.GetTime(in_date_t); //转为CTime型
CString in_date_CS = in_date_t.Format("%Y-%m-%d");	//转为CString型
std::string in_date_s = CT2A(in_date_CS.GetString());	//转为string型

CDateTimeCtrl in_time;	//时间
CTime in_time_t;
in_date.GetTime(in_time_t); //转为CTime型
CString in_time_CS = in_time_t.Format("%H:%M:%S");	//转为CString型
std::string in_time_s = CT2A(in_time_CS.GetString());	//转为string型
```
