---
layout: default
---

## [](header-2) 网格数据解析

> Obj格式网格数据的解析类

```cpp
// rcMeshLoaderObj

class rcMeshLoaderObj
{
private:
    void addVertex(float x, float y, float z, int& cap);
    void addTriangle(int a, int b, int c, int& cap);
    
    std::string m_filename;
    float m_scale;  
    float* m_verts;             // vb数组，size: m_vertCount*3
    int* m_tris;                // ib数组，size: m_triCount*3
    float* m_normals;
    int m_vertCount;            // 顶点数量
    int m_triCount;             // 三角形数量
};
```

> 读入数据后，构建每一个三角形数据相应的包围信息

```cpp
struct BoundsItem
{
    float bmin[2];  // bmin和bmax用于记录三角形i在XZ平面的包围矩形
    float bmax[2];
    int i;          // 三角形序号
};
```

> 代码中trisPerChunk为256，nchunks为所需chunk的数量，nodes则为长度为nchunks*4的rcChunkyTriMeshNode数组

```cpp
struct rcChunkyTriMesh
{
    inline rcChunkyTriMesh() : nodes(0), nnodes(0), tris(0), ntris(0), maxTrisPerChunk(0) {};
    inline ~rcChunkyTriMesh() { delete [] nodes; delete [] tris; }

    rcChunkyTriMeshNode* nodes;     // size: nchunks*4
    int nnodes;
    int* tris;                      // 排序后的三角形ib数据
    int ntris;                      // 三角形数量
    int maxTrisPerChunk;
};
```


```cpp
struct rcChunkyTriMeshNode
{
    float bmin[2];      // 所有子节点汇总的包围盒
    float bmax[2];      
    int i;              // i>0为叶子节点，表示rcChunkyTriMesh tris数组中的序号，i<0为中间控制节点
    int n;              // i>0时表示包含三角形的数量
};
```

```cpp
// @remark              递归地划分和重排三角形

// @param items         BoundItem数组
// @param nitems        数组的长度
// @param imin          待处理的最小三角形序号（包含）
// @param imax          待处理的最大三角形序号（不包含）
// @param trisPerChunk
// @param curNode       当前位于nodes数组的序号
// @param nodes         rcChunkyTriMeshNode数组，即三角形包围盒树
// @param maxNodes      nodes数组的最大长度
// @param curTri        当前位于输出三角形数组的序号
// @param outTris       输出的三角形数组
// @param inTris        输入的三角形数组
static void subdivide(BoundsItem* items, int nitems, int imin, int imax, int trisPerChunk,
                      int& curNode, rcChunkyTriMeshNode* nodes, const int maxNodes,
                      int& curTri, int* outTris, const int* inTris)
```

> subdivide将三角形逐步二分地进行重排，并构建一个汇总XZ平面上包围盒信息的二叉树

1. 


### [](header-3) 为什么是nchunks*4呢

> 以ntris=10133，trisPerChunks=256为例:
>
> _8192(256 * 32) < 10133 < 16384(256 * 64)_
>
> 因此，叶子节点最少需要64个，由于subdivide必然构造出完全二叉树，所以总的节点数需要128个
>
> 而 10133 / 256 * 2 > 16384 / 256，10133 / 256 < 16384 / 256，故此需要预留10133 / 256 * 4 = 160个节点，即nchunks*4个节点