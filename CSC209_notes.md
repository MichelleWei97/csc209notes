# CSC209H1 Winter2024 Notes #

​															(by Michelle Wei)

## [Supplementary](https://drive.google.com/drive/folders/1KOKT_DjfJpJ9e5qlm5b2ZVSrGi3va-iN?usp=sharing) (In Google Drive) ##

> **1. 思睿笔记，视频**
>
> > 1. Unix Command-Line, Shell Programming, File Permission, Function
> > 2. Pointer, Array, Memory Layout
> > 3. Memory Layout, File Handling
> > 4. String, Struct
>
> **2. Lecture Slides**
>
> **3. Worksheet**
>
> **4. Lab Files**
>
> **5. Past Tests**
>
> **6. Assignments **

## Week 1 ##

### Pointer ###

> 指针概况：
>
> > 基本语法：
>
> dereference：

### Array ###

> Array本质上是一种指针，他指向这个数组的第一个元素在内存中的位置

### Questions in this week

## Week 2 ##

### Command-line Arguments ###

### Questions in this week ###

### Files Intro ###

### Streams ###



## Week 3 ##

### Linked Structures and Iterations ###

> **Array** has fixed size  , **Linked Structure** has dynamic size.
>
> **Nodes** in linked structure contain its *data* and *pointer to next node*.
>
> 第一个node front, (sturct node *) 最后一个node point到null
>
> ```c
> struct node {
> int value;
> struct node *next;
> }
> typedef struct node Node;
> ```
>
> 在使用linkedlist的时候，首先建立一个空列表，front pointer为NULL，程序运行时，items 不断加入list。每加一个node时，我们allocate a new node，把这个node链接给list。我们一次只创建一个node，和array一次创建所有不太一样。
>
> ```c
> int main(){
> struct node *node_a = malloc(sizeof(struct node));
> // other hidden malloc calls
> struct node *node_b = malloc(sizeof(struct node));
> node_a->next = node_b;
> printf("node_a: %p\n", node_a);
> printf("node_b: %p\n", node_b);
> }
> ```
>
> **Basic traverse** of linked list:
>
> ```c
> #include <stdio.h>
> #include <stdlib.h>
> 
> typedef struct node {
>   int value;
>   struct node *next;
> //give the struct a new name, Node, 所以每次不用再打一遍struct
> }Node;
> 
> Node *create_node(int num, Node *next){
>   Node *new_node = malloc(sizeof(Node));
>   new_node->value = num;
>   new_node->next = next;
>   return new_node;
> }
> int main(){
>   Node *front = NULL;
>   front = create_node(3, front);
>   front = create_node(2, front);
>   front = create_node(1, front);
>   Node *curr = front;
>   while (curr != NULL){
>     printf("%d\n", curr->value);
>     curr = curr->next;
>   }
>   return 0;
> }
> ```
>
> **Insertion** into the middle of a list:
>
> ```c
> //如果想在第一个和第二个node之间插入一个node
> //1.create a new node (new_node)
> //2. new_node.next = node_2
> //3. connect node_1 to new_node
> void insert(int num, Node *front, int position){
>   Node *curr = front;
>   for (int i = 0; i < position - 1; i++){
>     curr = curr->next;
>   }
>   printf("Currently at %d\n", curr->value);
>   Node *new_node = create_node(num, curr->next);
>   curr->next = new_node;
> }
> ```
>
> **Testing Insertions**
>
> > Cases: midlle, beginning, illegal index
>
> Change the **insert** 因为要考虑插入到最后一个node的情况
>
> ```c
> int insert(int num, Node **front_ptr, int position){
>   //用Node **front_ptr是因为我们要操作ptr的地址，才能改变main里面front
>   Node *curr = *front_ptr;
>   if (position == 0){
>     // front = create_node(num, front); 不对是因为这一行只改变了函数里的front，没有改变真正main里的front，所以我们要操作main里面front的地址
>     *front_ptr = create_node(num, *front_ptr);
>     return 0;
>   }
>   
>   for (i = 0; i < position - 1 && curr != NULL; i++){
>     curr = curr->next;
>   }
>   if (curr == NULL){
>     return -1; // if failed
>   }
>   Node *new_node = crerate_node(num, curr->next);
>   curr->next = new_node;
>   return 0; //if successful
> }
> int main(){
>   Node *front = NULL;
>   front = create_node(4, front);
>   front = create_node(3, front);
>   front = create_node(1, front);
>   
>   insert(2, &front, 1);
>   insert(5, &front, 4); //insert at end
>   insert(9000, &front, 9000); //insert at invalid index
>   insert(0, &front, 0); //insert at the front
>   
>   Node *curr = front;
>   while (curr != NULL){
>     printf("%d\n", curr->value);
>     curr = curr->next;
>   }
>   return 0;
> }
> ```

### Dynamic memory and strings ###

### Struct ###

> Structs用于管理不全是一个type的data 
>
> |                     | Array                                                        | Structure                                                    |
> | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | data of same type   | Yes                                                          | Not required                                                 |
> | declaration details | type and number of <font color=#008000>elements</font><br />(array [] notation) | types of <font color=#008000>members</font><br />(struct keyword) |
> | access via ...      | index notation                                               | dot notation                                                 |
>
> ```c
> #include <stdio.h>
> #include <string.h>
> int main(void){
> struct student {
> char first_name[20];
> char last_name[20];
> int year;
> float gpa;
> };
> struct student good_student;
> strcpy(good_student.first_name, "Jo");
> strcpy(good_student.last_name, "Smith");
> // dot syntax used to access members of structs
> good_student.year = 2;
> good_student.gpa = 3.2;
> printf("Name: %s %s\n", good_student.first_name, good_student.last_name);
> printf("Year: %d. GPA: %d.2f\n", good_student.year, good_student.gpa);
> return 0;
> 
> }
> 
> ```
>
> How to use Struct as **function parameters**
>
> 对于*function*来说
>
> ```c
> #include <stdio.h>
> void change(int numbers[]){
> numbers[0] = 80;
> }
> int main(){
> int my_array[5];
> my_array[0] = 40;
> change(my_array);
> printf("Element at index 0: %d\n", my_array[0]);
> return 0;
> }
> /*
> > gcc -Wall array.c
> > ./a.out
> Element at index 0: 80
> 是因为array是一个pointer，change function把array的第一个element的地址上的值修改成了80。
> */
> ```
>
> 对于*struct*来说
>
> ```c
> #include <stdio.h>
> #include <string.h>
> struct student{
>   char first_name[20];
>   char last_name[20];
>   int year;
>   float gpa;
> };
> void change(struct student s){
>   s.gpa = 4.0;
>   strcpy(s.first_name, "Adam");
> }
> int main(){
>   struct student good_st;
>   strcpy(good_st.first_name, "Jo");
>   good_st.gpa = 3.2;
>   change(good_st);
>   printf("first name is %s\n", good_st.first_name);
>   printf("GPA is %f\n", good_st.gpa);
>   return 0;
> }
> /*
> > gcc -Wall structs2.c
> > ./a.out
> first name is Jo
> GPA is 3.2
> 没有修改的原因是 "A function gets a copy of the struct". Function change the copy of the struct, not the original. Function gets a copy of the entire struct, including *the array in the struct*
> */
> 
> ```
>
> 如果想要通过function改变struct里面的值，需要使用 **return**，或者直接pass struct的pointer给function。
>
> ```c
> #include <stdio.h>
> #include <string.h>
> struct student{
>   char first_name[20];
>   char last_name[20];
>   int year;
>   float gpa;
> };
> void change(struct student *s){
>   //有括号是因为dot的优先级比star高
>   (*s).gpa = 4.0;
>   strcpy((*s).first_name, "Adam");
> }
> int main(){
>   struct student good_st;
>   strcpy(good_st.first_name, "Jo");
>   good_st.gpa = 3.2;
>   change(&good_st);
>   printf("first name is %s\n", good_st.first_name);
>   printf("GPA is %f\n", good_st.gpa);
>   return 0;
> }
> ```
>
> ```c
> int main(){
>   struct student s;
>   struct student *p;
>   strcpy(s.first_name, "Jo");
>   strcpy(s.first_name, "Smith");
>   s.year = 2;
>   s.gpa = 3.2;
>   p = &s;
>   
>   //两种方法进行更改，使用dot或者使用箭头
>   (*p).gpa = 3.8;
>   p->year = 1;
>   
>   strcpy(p->first_name, "Henrick");
>   printf("first name is %s\n", s.first_name);
>   printf("GPA is %f\n", s.gpa);
>   return 0;
> }
> ```

### Strings ###

> **String** is a character array that has a null character after the final character of the text.
>
> 在C中，给string重新**赋值**要用**strcpy(s1, s2)**
>
> 最后的'\0'不显示，只是用于mark string的结尾
>
> ```c
> char text0[20] = {'h', 'e', 'l', 'l', 'o', '\0'};
> //简写，用double quote
> char text1[20] = "hello";
> //改变string的char
> text1[0] = 'j';
> //或者可以不写string的长度，但此时要initialize char是什么
> char text2[] = "abcde";
> //不可以 char text2[];
> printf("%s\n", text);
> ```
>
> **String literal** 是string constant，对str第一个元素的pointer
>
> ```c
> //不可以改变！！！
> char *text = "hello"
> ```
>
> sizeof **不能**反映str的长度，要用<string.h>里的**strlen**
>
> ```c
> #include <string.h>
> // strlen in string library 能得到str的长度
> size_t strlen(const char *s);
> //copy the s2 to s1
> //unsafe,因为无法确定s1的空间是否够用
> char *strcpy(char *s1, const char *s2);
> //safe,因为n确定了s1的空间, int n 是sizeof(s1),但有时候也不太行,因为遗漏了null pointer '\0'
> char *strncpy(char *s1, const char *s2, int n);
> ```
>
> safe way example:
>
> ```c
> #include <stdio.h>
> #include <string.h>
> int main(void){
>   char s1[5];
>   char s2[32] = "University of";
>   strncpy(s1, s2, sizeof(s1));
>   s1[4] = '\0';
>   printf("%s\n", s1);
>   printf("%s\n", s2);
>   return 0;
> }
> ```
>
> String Concatenation
>
> ```c
> //add s2 to the end of s1, not safe
> char *strcat(char *s1, const char *s2);
> //safe
> char *strncat(char *s1, const char *s2, int n);
> //使用方法
> int main(){
>   char s1[30];
>   char s2[14] = "University of";
>   char s3[15] = " C Programming";
>   strcpy(s1, s2);
>   //最多能copy sizeof(s1) - strlen(s1) - 1 (给'\0'留位置)
>   strncat(s1, s3, sizeof(s1) - strlen(s1) - 1);
> }
> ```
>
> Searching char and str inside the str
>
> ```c
> char *strchr(const char *s, int c);
> //return the pointer where the char is found, or NULL if not
> int main(){
>   char s1[30] = "University of C programming";
>   char *p;
>   p = strchr(s1, 'v');
>   if (p == NULL){
>     printf("Character not found.\n");
>   }else{
>     // p - s1得到index, 因为p指向当前位置, s1指向str的第一个元素
>     printf("Character is found at index %d\n", p - s1);
>   }
>   return 0;
> }
> 
> char *strstr(const char *s1, const char *s2);
> int main(){
>   char s1[30] = "University of C programming";
>   char *p;
>   p = strchr(s1, 'sity');
>   if (p == NULL){
>     printf("substring not found.\n");
>   }else{
>     // p - s1得到index, 因为p指向当前位置, s1指向str的第一个元素
>     printf("Character is found at index %d\n", p - s1);
>   }
>   return 0;
> }
> ```

### Questions in this week ###

```c
#include <stdio.h>
#include <string.h>

//Q1 What is the output? ANS: University
int main() {
    char s1[30] = "University of C Programming";
    char *p;
    p = strchr(s1, ' ');
    if (p != NULL) {
        *p = '\0';
    }
    printf("%s\n", s1);
    return 0;
}
//Q2 What is printed? ANS: Unknown; it depends on the size of a char pointer.
char *name = "Daniel";
printf("%lu\n", sizeof(name));
```



## Week 4 ##

### Compiling ###

> **The Compiler Toolchain**: source code(.c) -----> executable(.out) -----> executing program
>
> gcc(compile) ./a.out(run) 
>
> 

### Low-Level I/O ###

### Files ###

### Questions in this week ###



## Week 5 ##



