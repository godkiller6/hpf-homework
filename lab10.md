**高数**

1.求极限

例如需要求以下的极限：

![
](http://p1.pstatp.com/large/pgc-image/1539761880190f25253589d)

实现代码如下：

    from sympy import *
    def qiujixain():
     x = symbols('x')
     expr = x**2/exp(x)
     # 无穷用两个o表示
     result = limit(expr,x,oo)
     print('被求函数为：')
     pprint(expr)
     print('结果为：')
     pprint(result)
    if __name__ == '__main__':
     qiujixain()

代码中limit函数用于求极限，第一个参数放表达式，第二个为自变量，第三个为表达式在某处的极限。输出结果如下：

![
](http://p3.pstatp.com/large/pgc-image/1539761880219a94e4b891e)

2.微分

求微分用到diff()函数，例如需要求以下微分：

![
](http://p1.pstatp.com/large/pgc-image/15397618802301cc2f9f2d0)

实现代码如下：

    from sympy import *
    def qiuweifen():
     """求微分"""
     f = Function('f')
     x = symbols('x')
     f = exp(tan(1/x)) * sin(1/x)
     result = diff(f,x)
     print('被求函数为')
     pprint(f)
     print('结果为:')
     pprint(result)
    if __name__ == '__main__':
     qiuweifen()
     

diff()函数第一个参数存放表达式，第二个参数存放对哪个变量求微分，如需要计算高阶导数，可在后面加数字，没写默认求一阶导数。输出结果如下：

![
](http://p3.pstatp.com/large/pgc-image/15397618803792d425c6f6d)

**线代**、

1.二阶行列式

![
](https://img-blog.csdn.net/20180305001455765)

这是一个简单的二元线性方程组，我们调用linalg. solve()函数就可以快速解决

    import numpy as np
    a = np.array([[3, -2], [2, 1]])
    b = np.array([12, 1])
    d = np.linalg.solve(a, b)
    print(d)
    [ 2. -3.]
    #注意矩阵的逆和矩阵的转置的区别，此函数只对可逆矩阵有效果，如果不可逆则会出错。

2.全排列和对换

![
](https://img-blog.csdn.net/20180305002750763?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYm9keV9idWlsZGVy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

写一个简单的小循坏算法就可以解决

    a = str(32514)
    num = 0
    for i in range(len(a)):
        for j in range(i):
            if a[j]>a[i]:
                num+=1
    print(num)
    5
