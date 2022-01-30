http://poi.apache.org/components/spreadsheet/index.html

# 依赖



# Workbook 操作excel文件的接口

1. HSSFWorkbook xls格式的excel文件，一张表最大支持65536行数据，256列，一个sheet页，最多导出6w多条数据

   ```
   Workbook workBook = new HSSFWorkbook(inputStream);
   ```

2. XSSFWorkbook xlsx格式的excel文件，一张表最大支持1048576行，16384列

   ```
   Workbook workBook = new XSSFWorkbook(inputStream);
   ```

3. SXSSFWorkbook xlsx格式的excel文件，内存占用低，适合处理大数据量的表格（参见 https://www.cnblogs.com/visec479/p/4168686.html）

   ```
   Workbook workBook = new XSSFWorkbook(inputStream);
   ```



# Sheet 操作工作表的接口

```java
Sheet sheet = workBook.getSheetAt(index); //根据工作表索引获取表格
Sheet sheet = workBook.getSheet("表格名称");//根据工作表名称获取表格
int firstRowNum  = sheet.getFirstRowNum();//获得当前sheet的开始行索引
int lastRowNum = sheet.getLastRowNum();//获得当前sheet的结束行索引
int cellSize = sheet.getPhysicalNumberOfRows();//获取表格物理行数,不包括那些空行（隔行）的情况。
```



# Row 操作数据行的接口

```java
Row row = sheet.getRow(rowNum);//获取指定行数据
int firstCellNum = row.getFirstCellNum();//获得行的第一个有值的单元的索引,eg第一个单元格中有值,返回0,第二个单元格中有值,返回1
int lastCellIndex = row.getLastCellNum();//获得行的最后一个单元格的索引+1的值
int cellSize = row.getPhysicalNumberOfCells();//获取行的物理单元格数量,不包括那些空单元格的情况。
```



# Cell 操作单元格的接口

```java
Cell cell = row.getCell(cellNum);//根据单元格索引获取单元格
switch (cell.getCellType()){//获取单元格中不同类型数据
    case Cell.CELL_TYPE_NUMERIC: //数字
        cellValue = String.valueOf(cell.getNumericCellValue());
        break;
    case Cell.CELL_TYPE_STRING: //字符串
        cellValue = String.valueOf(cell.getStringCellValue());
        break;
    case Cell.CELL_TYPE_BOOLEAN: //Boolean
       cellValue = String.valueOf(cell.getBooleanCellValue());
       break;
    case Cell.CELL_TYPE_FORMULA: //公式
       cellValue = String.valueOf(cell.getCellFormula());
       break;
    case Cell.CELL_TYPE_BLANK: //空值 
       cellValue = "";
       break;
    case Cell.CELL_TYPE_ERROR: //故障
        cellValue = "非法字符";
        break;
    default:
        cellValue = "未知类型";
        break;
}
```



# 单元格添加超链接

https://blog.csdn.net/u010689306/article/details/52091123

```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet("Hyper Links");
XSSFCell cell;
CreationHelper createHelper = workbook.getCreationHelper();

// 创建超链接的样式
XSSFCellStyle hlinkstyle = workbook.createCellStyle();
XSSFFont hlinkfont = workbook.createFont();
hlinkfont.setUnderline(XSSFFont.U_SINGLE);
hlinkfont.setColor(HSSFColor.BLUE.index);
hlinkstyle.setFont(hlinkfont);

// 链接到一个网址 
cell = sheet.createRow(1).createCell((short)1);
cell.setCellValue("URL Link");
XSSFHyperlink  hyperLink = (XSSFHyperlink) createHelper.createHyperlink(Hyperlink.LINK_URL);
hyperLink.setAddress("http://www.yiibai.com");
cell.setHyperlink(hyperLink);
cell.setCellStyle(hlinkstyle);

// 链接到另一个文件
cell = sheet.createRow(2).createCell(1);
cell.setCellValue("File Link");
hyperLink = (XSSFHyperlink) createHelper.createHyperlink(Hyperlink.LINK_FILE);
hyperLink.setAddress("cellstyle.xlsx"); // 这里写文件路径，可以是相对路径或绝对路径，本例中 链接到当前目录下另一个文件
cell.setHyperlink(hyperLink);
cell.setCellStyle(hlinkstyle);

// e-mail 链接
cell = sheet.createRow(3).createCell((short) 1);
cell.setCellValue("Email Link");
hyperLink = (XSSFHyperlink) createHelper.createHyperlink(Hyperlink.LINK_EMAIL);
hyperLink.setAddress("mailto:contact@yiibai.com?"+"subject=Hyperlink");
cell.setHyperlink(hyperLink);
cell.setCellStyle(hlinkstyle);

// 链接到当前excel中另一个地方
cell = sheet.createRow(4).createCell((short) 1);
cell.setCellValue("Document Link");
hyperLink = (XSSFHyperlink) createHelper.createHyperlink(HyperlinkType.DOCUMENT);
hyperLink.setAddress("#sheet2!A1"); // #后面写工作表的名字 !后面写单元格
cell.setHyperlink(hyperLink);
cell.setCellStyle(hlinkstyle);

FileOutputStream fos = new FileOutputStream(new File("hyperLink.xlsx"));
workbook.write(fos);
fos.close();
System.out.println("hyperlink.xlsx written successfully");
```

