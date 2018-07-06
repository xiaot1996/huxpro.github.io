---
layout:     post
title:      "Java POI开发小结"
subtitle:   "用POI操作Excel文件"
date:       2018-07-02
author:     "Xiaotong CHEN"
header-img: 
catalog:
tags:
    - POI
    - Java
---
* 目录
{:toc}

# 关于POI以及一些想法
Apache POI是Apache软件基金会提供的100％开源库。大多数中小规模的应用程序开发主要依赖于Apache POI（HSSF+ XSSF）。它支持Excel 库的所有基本功能; 然而，呈现和文本提取是它的主要特点。
[官网链接](http://poi.apache.org/guidelines.html)

Java Excelapi有很多，除了Apahce POI，网上教程较多的Jexcel等。虽然没有使用过其他java excelapi，就本人使用Apahce POI开发感受和其他人的提醒，能用VB操作excel就尽量不要用java了。

Apache POI提供的基本功能，简单的读写ｅｘｃｅｌ文档没有问题，但是没有实现复制单元格(cell)，复制电子表格(Sheet)等功能（也有可能提供了但我没找到，毕竟没有很详细地完整看完官方Guide）。学习基本功能和了解POI实现机制，可以看上面官网链接或[易百中文教程](https://www.yiibai.com/apache_poi/apache_poi_core_classes.html)。

我在下面会列出一些我在开发时自己编写的ＰＯＩ工具类中一些常用函数，具体代码可以在我的GitHub [ApachePOI](https://github.com/xiaot1996/ApachePOI)项目中找到。


# 代码实现
## 获取有效行数
```
/********************************************************
	 * function : count the number of valid rows in a given sheet 
	 * *******************************************************
	 * parameters:
	 * wb : workbook which contains the sheet we want
	 * indexSheet : the index of the sheet in the xb (start from 0)
	 *************************************************************** */
	public static int getSheetRowNumber(XSSFWorkbook wb,int indexSheet)
	{
		XSSFSheet sheet=wb.getSheetAt(indexSheet);
		int count=0;
		int begin = sheet.getFirstRowNum();  
		  
	    int end = sheet.getLastRowNum();  
	  
	    for (int i = begin; i <= end; i++) {  
	        if (null == sheet.getRow(i)|| getCellValue(sheet.getRow(i).getCell(0)) == "" || null==sheet.getRow(i).getCell(0)) {  
	            continue;  
	        }  
	        else count++;
	    }
	    
	    return count;
	}
	
```

## 列编号(ABC...)与数字(123...)的转换
```
	 /**********************************************************
     * Excel column index begin 1
     * @param colStr
     * @param length
     * @return
     **********************************************************/
    public static int excelColStrToNum(String colStr, int length) {
        int num = 0;
        int result = 0;
        for(int i = 0; i < length; i++) {
            char ch = colStr.charAt(length - i - 1);
            num = (int)(ch - 'A' + 1) ;
            num *= Math.pow(26, i);
            result += num;
        }
        return result;
    }

    /**
     * Excel column index begin 1
     * @param columnIndex
     * @return
     */
    public static String excelColIndexToStr(int columnIndex) {
        if (columnIndex <= 0) {
            return null;
        }
        String columnStr = "";
        columnIndex--;
        do {
            if (columnStr.length() > 0) {
                columnIndex--;
            }
            columnStr = ((char) (columnIndex % 26 + (int) 'A')) + columnStr;
            columnIndex = (int) ((columnIndex - columnIndex % 26) / 26);
        } while (columnIndex > 0);
        return columnStr;
    }
```

## 在不同工作簿(workbook)间，复制一个区域的单元格并保留格式，设置行宽
```
/********************************************************
	 * function : copy a part of cells in different workbooks
	 * *******************************************************
	 * @param:
	 * xbIn : the resource workbook
	 * xbOut : the destination workbook
	 * indexSheetIn : the index of the sheet in the xbIn (start from 0)
	 * indexSheetOut : the index of the sheet in the xbOut (start from 0)
	 * rowInStart : the number of the first row that we need in the indexSheetIn ( start from 1 )
	 * rowInEnd : the number of the last row that we need in the indexSheetIn ( start from 1 )
	 * colInStart : the number of the first column that we need in the indexSheetIn ( start from 1, represent A in the sheet )
	 * colInEnd : the number of the last column that we need in the indexSheetIn ( start from 1, represent A in the sheet )
	 * rowOutStart : the number of the first row that we want to put cells in the indexSheetOut ( start from 1 )
	 * colOutStart : the number of the first column that we want to put cells in the indexSheetOut ( start from 1, represent A in the sheet )
	 * *******************************************************/
	public static boolean copyCells(XSSFWorkbook wbIn,XSSFWorkbook wbOut,int indexSheetIn,int indexSheetOut, 
			int rowInStart,int colInStart,int rowInEnd,int colINEnd,int rowOutStart,int colOutStart )
	{
		XSSFSheet sheetIn=wbIn.getSheetAt(indexSheetIn);
		XSSFRow rowIn;
		XSSFCell cellIn;
		XSSFCellStyle cellStyleIn;
		String cellInValue;
		
		
		XSSFSheet sheetOut=wbOut.getSheetAt(indexSheetOut);
		XSSFRow rowOut;
		XSSFCell cellOut;
		XSSFCellStyle cellStyleOut;
		
		int rowNum=rowInEnd-rowInStart;
		int colNum=colINEnd-colInStart;
		for(int i=0;i<rowNum+1;i++)
	    {  	    	
	    	
	    	rowIn=sheetIn.getRow(rowInStart+i-1); 		    		
	    	rowOut = sheetOut.createRow(rowOutStart+i-1);
			//set the copied row's height
	    	rowOut.setHeight(rowIn.getHeight());
	    	for(int j=0;j<colNum+1;j++) {
	    		cellIn=rowIn.getCell(colInStart+j-1);
	    		if(cellIn!=null) {
	    		cellInValue=getCellValue(cellIn);
	    	    cellOut=rowOut.createCell(colOutStart+j-1);
	    	    cellStyleIn=cellIn.getCellStyle();
	    	    cellStyleOut=wbOut.createCellStyle();
	    	    cellStyleOut.cloneStyleFrom(cellStyleIn);
	    	    cellOut.setCellStyle(cellStyleOut);
	    	    cellOut.setCellValue(cellInValue);
	    		}
	    	}	
	    }
		
		//in order to deal with merged regions
		java.util.List<CellRangeAddress> regions=sheetIn.getMergedRegions();
		
		for(CellRangeAddress cellRangeAddress : regions)
		{
			if(cellRangeAddress.getFirstColumn()>=colInStart-1&&
					cellRangeAddress.getLastColumn()<=colINEnd-1&&
					cellRangeAddress.getFirstRow()>=rowInStart-1&&
					cellRangeAddress.getLastRow()<=rowInEnd-1)
			{
				int diffrow=rowOutStart-rowInStart;
				int diffcol=colOutStart-colInStart;
			int firstRow=cellRangeAddress.getFirstRow()+diffrow;
			int firstCol=cellRangeAddress.getFirstColumn()+diffcol;
			int lastRow=firstRow+cellRangeAddress.getLastRow()-cellRangeAddress.getFirstRow();
			int lastCol=firstCol+cellRangeAddress.getLastColumn()-cellRangeAddress.getFirstColumn();
			CellRangeAddress cellRangeAddressNew=new CellRangeAddress(firstRow, lastRow, firstCol, lastCol);

			sheetOut.addMergedRegion(cellRangeAddressNew);
			}
		}
		//set the copied column's width
		for(int columnIndex=colInStart-1;columnIndex<=colINEnd-1;columnIndex++) {
			int width=sheetIn.getColumnWidth(columnIndex);
			sheetOut.setColumnWidth(columnIndex, width);
		}
		
	    return true;
	}
```

# 需要注意的
- 在linux环境下，使用LibreOffice Calc，虽然在给每个电子表格(sheet)命名时，无论代码中还是软件中，名称可以包含空格或特殊字符，但用apahce poi获取sheet名时，只会读到空格或特殊符号之前的部分。我的解决方法是在创建每个sheet时统一用横杠(-)代替空格和特殊字符。

- 对于合并单元格(merged region)，好的做法是在**同一个函数**中先把该表中**所有**要合并的单元格存到cellRangeAddress列表中，再进行复制之类的操作，避免在多个函数反复添加合并单元格。

- 自动设置列宽方法autoSizeColumn，对于合并单元格不能很好适用，建议还是自己设置。

- 在设置格式时，LibreOffice Calc中形如`=$表名.单元格编号`。但在java POI中形式为`cell.setcellformula(表名!单元格编号)`


