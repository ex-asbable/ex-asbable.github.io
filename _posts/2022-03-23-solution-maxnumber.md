---
layout: post
title:  "20220322校内考试 T1 最大数 题解"
categories: 题解
tags: 校内
author: ex_Asbable
---

* content
{:toc}

const mathBlocks = document.querySelectorAll('script[type^="math/tex"]');
Array.from(mathBlocks).forEach((el) => {
  const tex = el.textContent.replace("% <![CDATA[", "").replace("%]]>", "");
  el.outerHTML = window.katex.renderToString(tex, {
    displayMode: el.type === "math/tex; mode=display",
  });
});

## 20220322校内考试 T1 最大数 题解

### 题目描述

现在请求你维护一个数列，要求提供查询和修改两种操作：

1.查询数列中最后 $$L$$ 个数的最大值

2.将数列末尾插入一个数，![](http://latex.codecogs.com/gif.latex?(n+t)\ mod\ d)，t为最近一次查询操作的答案，强制在线

### 解法

单点修改，区间查询。

可以想到线段树。

首先建树，范围1到m，设一个数来记录当前数列长度，添加元素时在长度+1的地方增加那个数，并使长度+1；查询时直接区间查询即可。

### CODE

```cpp
const ll mn=2e5+10;
const ll mm=2e5+10;
const ll mod=1e9+7;
const ll inf=0x3f3f3f3f;
const ll fni=0xc0c0c0c0;
ll n,d;
struct stree{
    ll l,r;
    ll add,dat;
    #define l(x) tr[x].l
    #define r(x) tr[x].r
    #define add(x) tr[x].add
    #define dat(x) tr[x].dat
}tr[mn<<2];
#define lc(x) (x<<1)
#define rc(x) (x<<1|1)
void build(ll now,ll l,ll r){
    l(now)=l;r(now)=r;
    if(l==r)return;
    ll mid=(l+r)>>1;
    build(lc(now),l,mid);
    build(rc(now),mid+1,r);
}
inline void spread(ll now){
    if(!add(now))return;
    dat(lc(now))=max(dat(lc(now)),add(now));
    dat(rc(now))=max(dat(rc(now)),add(now));
    add(lc(now))=max(add(lc(now)),add(now));
    add(rc(now))=max(add(rc(now)),add(now));
    add(now)=0;
}
void plu(ll now,ll l,ll r,ll o){
    if(l<=l(now) && r>=r(now)){
        dat(now)=max(dat(now),o);
        return;
    }
    spread(now);
    ll mid=(l(now)+r(now))>>1;
    if(l<=mid)plu(lc(now),l,r,o);
    if(r>mid)plu(rc(now),l,r,o);
    dat(now)=max(dat(lc(now)),dat(rc(now)));
}
ll ask(ll now,ll l,ll r){
    if(l<=l(now) && r>=r(now))return dat(now);
    spread(now);
    ll mid=(l(now)+r(now))>>1,ans=0;
    if(l<=mid)ans=max(ans,ask(lc(now),l,r));
    if(r>mid)ans=max(ans,ask(rc(now),l,r));
    return ans;
}
inline void read_init(){
    n=read<ll>();d=read<ll>();
    ll las=0,m=0;
    build(1,1,n);
    for(ll i=1;i<=n;i++){
        char s[5];
        scanf("%s",s+1);
        ll o=read<ll>();
        if(s[1]=='A')m++,plu(1,m,m,(o+las)%d);
        else las=ask(1,m-o+1,m),write(las,1);
    }
}
int main(){
    freopen("maxnumber.in","r",stdin);
    freopen("maxnumber.out","w",stdout);
    read_init();
    
    return 0;
}
```

