lightHouse
==========

PA1-2灯塔问题，结构体排序，归并排序，逆序对，结构体排序的排序函数写法

灯塔的坐标x, y是整数，且不同灯塔的x, y坐标均互异

1 ≤ x, y ≤ 10^7

提醒
注意机器中整型变量的范围，C/C++中的int类型通常被编译成32位整数，其范围为[-231, 231 - 1]，不一定足够容纳本题的输出。

时间：2s，内存：256MB

相关代码：

\#include \<iostream>

\#include \<cstring>

\#include \<cmath>

\#include \<cstdlib>

\#include \<algorithm>

\#include \<vector>

using namespace std;

//将两个有序数列a[left,mid]和a[mid,right]进行合并

int mergeArray(int a[], int left, int mid, int right, int temp[]);

int mergeSort(int a[], int left, int right, int temp[]);

struct Point{

    int x;
    int y;
};//灯塔的横坐标和纵坐标

bool lessMark(const Point & p1, const Point & p2){

    return p1.x < p2.x;//让结构体以横坐标的升序排序
}

bool greaterMark(const Point & p1, const Point & p2){

    return p1.x > p2.x;//让结构体以横坐标的降序排序
}



int main(){
    
    int n = 0;//灯塔的个数
    
    while(cin>>n){
        int arr[100000000] = {0};//初始化，存纵坐标y
        int temp[10000000000] = {0};//中转数组
        vector<Point> v;//存放结构体的向量
        for(int i = 0; i < n; i++){
            Point p;
            cin>>p.x>>p.y;
            v.push_back(p);
        }
        sort(v.begin(),v.end(),lessMark);
        for(int j = 0; j < v.size(); j++){
            arr[j] = v[j].y;//按照x排序的y已经放到数组中，只要求其中的逆序对即可
        }
        int result = mergeSort(arr,0,n-1,temp);
        cout<<result<<endl;
        v.clear();
        
    }

    //
    return 0;
}

int mergeArray(int a[], int left, int mid, int right, int temp[]){
    
    int i = left;
    int j = mid + 1;
    int m = mid;
    int n = right;
    int k = 0;
    int count = 0;//计算逆序对的个数
    
    while(i <= m && j<= n){
        if(a[i] <= a[j]){
            temp[k++] = a[i++];
        }else{
            temp[k++] = a[j++];
            count += m - i;//计算逆序对
        }
    }
    
    while(i <= m){//如果左边有剩余元素，则都是大的，直接接到队尾
        temp[k++] = a[i++];
    }
    
    while(j <= n){//如果右边有剩余元素，则都是大的，直接接到队尾
        temp[k++] = a[j++];
    }
    
    for(int r = 0; r < k ; r++){
        a[left + r] = temp[r];//得到合并后的数组
    }
    
    return count;
}

int mergeSort(int a[], int left, int right, int temp[]){

    if(left < right){//此处无等号，就一个元素就不用排序
        int mid = (left +right)/2;
        mergeSort(a,left,mid,temp);//左边有序
        mergeSort(a,mid+1,right,temp);//右边有序
        int result = mergeArray(a,left, mid, right, temp);//将两个有序的合并
        return result;
    }
    return -1;
}
