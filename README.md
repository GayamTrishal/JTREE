# JTREE
# Question
Nick lives in a country named JosephLand. JosephLand consists of N cities. City 1 is the capital city. There are N - 1 directed roads. It's guaranteed that it is possible to reach capital city from any city, and in fact there is a unique path from any city to the capital city.

Besides, you can't cross roads for free. To pass a road, you must have a ticket. There are total M tickets. You can not have more than one ticket at a time! Each ticket is represented by three integers:

v k w : you can buy a ticket with cost w in the city v. This ticket can be used at max k times. That means, after using this ticket for k roads ticket can't be used!
By the way, you can tear your ticket any time and buy a new one. But you are not allowed to buy a ticket if you are still having a ticket with you!

Nick's home is located in the capital city. He has Q friends, and he wants to invite all of them for dinner! So he is interested in knowing about how much each of his friends is going to spend in the journey! His friends are quite smart and always choose a route to capital city that minimizes his/her spending! Nick has to prepare dinner, so he doesn't have time to figure out himself, Can you please help him?

Please note that it's guaranteed that, one can reach the capital from any city using the tickets!

Input

The first line of input contains two space separated integers N and M, denoting the number of cities in JosephLand, and the number of tickets, respectively.

The next N - 1 lines contain two integers ai, bi, denoting that there is a road from city ai to bi.

The next M lines contain three integers vi, ki, wi denoting the ticket described above.

The next line contains a single integer Q denoting the number of Nick's friends.

The next Q lines contain a single integer each, hi denoting city, where Nick's ith friend lives.

Output

Output Q lines: the answer for each friend.

Constraints

    1 ≤ N, M, Q ≤ 105 
    1 ≤ vi, ki ≤ N
    1 ≤ wi ≤ 109
    1 ≤ hi ≤ N

Example

Input:

    7 7
    3 1
    2 1
    7 6
    6 3
    5 3
    4 3
    7 2 3
    7 1 1
    2 3 5
    3 6 2
    4 2 4
    5 3 10
    6 1 20
    3
    5
    6
    7

Output:

    10
    22
    5
    
Subtasks

    Subtask #1 (10 points) : N, M, Q ≤ 102, wi ≤ 105
    Subtask #2 (15 points) : N, M, Q ≤ 103, wi ≤ 108
    Subtask #3 (25 points) : N, M, Q ≤ 5*104, wi ≤ 109
    Subtask #4 (50 points) : Original constraints
    
Explanation

Example #1:

Query 1. There is only one ticket he/she can purchase in city 5 and it can be used 3 times. It's enough to reaching capital, thus overall cost is 10.

Query 2. There is only one ticket he/she can purchase in city 6 and it can be used only one time. Thus, he/she have to purchase another ticket in city 3 with cost 2. Overall cost is 22.

Query 3. There are two tickets he/she can purchase in city 7. One can buy either 2nd ticket or 1st ticket. He/She can buy the first ticket and travel, and buy one more ticket at city 3 by paying a cost of 2. Total cost for this combination is 5. For all other combination of buying tickets, he/she will have to spend at least 5 cost.

******************************************************************************************************************************

#Solution

    #include <iostream>
    #include <fstream>
    #include <cmath>
 
    using namespace std;
 
    #define maxn 1000000000000000000
 
    long long int n,m,v,i,u,q,y,k,w;
    long long int money[100001],len[100001],min_[100001],ko[100001],s[100001],count[100001],wi[100001];
 
    template <typename T>
    struct node{
                T data,k,w;
                node *right,*left;
                };
 
    template <class T>
    class list{
                    node<T> *front,*rear;
                    public:
                    void push_back(T,T,T);
                    void clear();
                    void check(T,T);
                    void goloop(T,T);
                    list()
                    {
                        front=NULL;
                    }
                };
    list<long long int> A[100001],B[100001];
    template <class T>
    void list<T>::push_back(T a,T b,T c)
    {
        node<T> *add;
        add=new struct node<T>;
        add->data=a;
        add->k=b;
        add->w=c;
        add->right=NULL;
        add->left=NULL;
        if(front==NULL)
        {
            front=add;
            rear=add;
        }
        else
        {
            rear->right=add;
            add->left=rear;
            rear=add;
        }
    }
    template <class T>
    void list<T>::check(T p,T x)
    {
        node<T> *ptr1;
        long long int i,prevf=0,nowf,previ=x;
        ptr1=front;
        if(money[p]!=maxn);
        else if(ptr1==NULL);
        else
        {
            while(ptr1!=NULL)
            {
                prevf=0;
                previ=x;
                if(ptr1->k>=len[p])
                    money[p]=min(money[p],ptr1->w);
                else if(ptr1->w<money[p])
                {
                    for(i=x;i>=0;i--)
                    {
                        if((ptr1->k>=(len[p]-s[i]))&&(ptr1->k<=(ko[i]+(len[p]-s[i]))))
                        {
                            money[p]=min(money[p],min_[i]+ptr1->w);
                            break;
                        }
                        else
                        {
                            nowf=(ko[i]+(len[p]-s[i]));
                            if(prevf>nowf)
                                count[previ]++;
                            previ=i;
                            i-=count[i];
                            prevf=nowf;
                        }
                    }
                }
                ptr1=ptr1->right;
            }
        }
    }
    template <class T>
    void list<T>::goloop(T p,T x)
    {
        node<T> *ptr;
        long long int z,l=0,k;
        ptr=front;
        if(ptr==NULL)
            return;
        if(money[p]!=maxn)
        {
            min_[x]=money[p];
            s[x]=len[p];
            wi[x]=0;
            count[x]=0;
            z=x-1;
            while((z>=0)&&(min_[z]>min_[x]))
            {
                ko[x]+=ko[z]+1;
                wi[x]+=wi[z]+1;
                z-=(wi[z]+1);
                count[x]++;
            }
        }
        else
        {
            l=1;
            ko[x]++;
        }
        k=ko[x];
        while(ptr!=rear)
        {
            if(l==0)
            {
                len[ptr->data]=len[p]+1;
                ko[x+1]=0;
                B[ptr->data].check(ptr->data,x);
                A[ptr->data].goloop(ptr->data,x+1);
            }
            else
            {
                len[ptr->data]=len[p]+1;
                B[ptr->data].check(ptr->data,x-1);
                A[ptr->data].goloop(ptr->data,x);
            }
            ko[x]=k;
            ptr=ptr->right;
        }
        if(l==0)
        {
            ko[x+1]=0;
            len[ptr->data]=len[p]+1;
            B[ptr->data].check(ptr->data,x);
            A[ptr->data].goloop(ptr->data,x+1);
        }
        else
        {
            len[ptr->data]=len[p]+1;
            B[ptr->data].check(ptr->data,x-1);
            A[ptr->data].goloop(ptr->data,x);
        }
    }
    template <class T>
    void list<T>::clear()
    {
        node<T> *ptr;
        ptr=front;
        while(ptr!=rear)
        {
            node<T> *del;
            del=ptr;
            ptr=ptr->right;
            delete del;
        }
        delete ptr;
        front=NULL;
        rear=NULL;
    }
 
    int main()
    {
        //ifstream cin;
        //cin.open("input.c++");
        ios_base::sync_with_stdio(false);
        cin>>n>>m;
        //cout<<n<<m;
        for(i=0;i<(n-1);i++)
        {
            cin>>u>>v;
            A[v].push_back(u,0,0);
        }
        for(i=1;i<=n;i++)
        {
            ko[i]=0;
            money[i]=maxn;
        }
        ko[0]=0;
        money[1]=0;
        for(i=0;i<m;i++)
        {
            cin>>v>>k>>w;
            B[v].push_back(0,k,w);
        }
        A[1].goloop(1,0);
        cin>>q;
        while(q--)
        {
            cin>>y;
            cout<<money[y]<<"\n";
        }
        for(i=1;i<=n;i++)
        {
            A[i].clear();
            B[i].clear();
        }
        //cin.close();
        return 0;
    }
