# 05 神奇的POI
复习：
VO：查询
PO：持久化

## POI
JAVA语言操作excel主流
1. POI Apache 他是用来操作Office所有软件的

早期Office的excel是二进制文件
后期是OOXML文档结构，现在的excel文件实际上是一个xml格式文件。

### 7个步骤
正常excel文件创建
1. 创建Excel文件---工作薄
2. 工作薄自动创建了三个工作表
3. 定位行
4. 定位列，获得单元格
5. 填写内容
6. 修饰
7. 由内存中保存到硬盘

POI创建Excel文件
```
    //有格式的
    @Test
    public void HSSFStyle() throws IOException {
        //1.创建一个工作蒲
        Workbook workbook = new HSSFWorkbook();
        //2.创建一个sheet
        Sheet sheet = workbook.createSheet("default");
        //3.创建行
        Row nRow = sheet.createRow(5);
        //4.创建单元格（列）
        Cell cell = nRow.createCell(5);
        //5.设置内容、格式
        cell.setCellValue("i love you");
        //从工作蒲中创建cellStyle
        CellStyle fontStyle = workbook.createCellStyle();
        Font font = workbook.createFont();
        font.setFontName("Open Sans Extrabold");//设置字体
        font.setFontHeightInPoints((short)26);//像素大小，short类型
        font.setColor((short) 128);
        fontStyle.setFont(font);
        cell.setCellStyle(fontStyle);
        //6.创建流，保存
        OutputStream outputStream = new FileOutputStream(new File("/run/media/hwolf/ubuntu/test2.xls"));
        workbook.write(outputStream);
        //7.关闭
        outputStream.close();
        workbook.close();
    }
```
代码存在的问题：
1. POI对象都在内存中
2. 重复创建对象，减少代码两
3. 行、列对象的复用，字体不行，因为是整体的，所以用不同的需要new一个。

出货表的开发步骤：
1. 货物数据
2. 创建excel文件
3. 将数据写入excel
4. 下载文件

## 05-04 POI存在的问题、创建出货表
我们要为Excel创建很多样式的，很复杂，麻烦。
 
 > 年月日格式化控件，自己去找

 sql用正则表达式过滤 

一个对象是一行，一列是属性
创建过多的字体POI可能保存不了。所以只能利用方法创建部分，然后调用函数，将其作为参数传入。

除了设置字体，也可以设置行列宽
也可以设置打印的宽度

```
//拿模版样式，放在webapp目录下，当作资源
POI可以有一切excel你能想到的操作。
```

### 下载的工具类
上传下载的工具类

## 05-06 模版调用
```
//linux 下jdk1.8方法获取时，不会自己拼写目录
String path = request.getSession().getServletContext().getRealPath("/") + "/make/xls/";
InputStream is = new FileInputStream(new File(path+"test.xls"));
```
遇见乱码，反复尝试。

## POI的升级---HSSF->XSSF
POI百万数据打印
HSSF---.xls
XSSF---.xlsx
1. 升级对象
2. 升级模版
3. 更改后缀

## 05-08 POI百万数据打印
1. 从数据库读取数据，LIST在构造时候十分耗费内存和CPU
2. xlsx一个单sheet可以支持1048576条数据，

POI意识到问题，高版本解决了海量数据导出
JDK提供的监控内存的
jvisualvm.exe 
处理大数据的
poi-ooxml-schemas
它的解决思路：
在打印过程中，已经加工完的对象临时存在一个文件中，文件采用xml格式。最终处理完时，将这些临时内容写入到最终的xlsx文件中。
1. 数据库读取
分次读取，不然会卡主
2. 每多少条数据写一次，清空缓存--SXSSFwork
3. 写在临时目录下
4. 但是这个对象不能使用模版开发，只能用于大数据的迁移
### 05-09csv时一个标准导入导出的文件，可以导入导出数据库中
1. SQLyong 







