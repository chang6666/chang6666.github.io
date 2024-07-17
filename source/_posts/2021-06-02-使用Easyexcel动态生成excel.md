---
title: 使用Easyexcel动态生成excel
date: 2021-06-02 08:29:53
categories:
- Java
tags:
- Java
---

#### 1. EasyExcel简介

EasyExcel是一个基于Java的简单、省内存的读写Excel的开源项目。在尽可能节约内存的情况下支持读写百M的Excel。 github地址: https://github.com/alibaba/easyexcel

#### 2. 使用方法

##### 添加依赖

~~~java
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>2.2.11</version>
        </dependency>
~~~



##### 设置excel默认样式

~~~java
public static HorizontalCellStyleStrategy defaultStyles() {
        //TODO 默认样式
        //表头样式策略
        WriteCellStyle headWriteCellStyle = new WriteCellStyle();
        //设置表头居中对齐
        headWriteCellStyle.setHorizontalAlignment(HorizontalAlignment.CENTER);
        //表头前景设置淡蓝色
        headWriteCellStyle.setFillForegroundColor(IndexedColors.PALE_BLUE.getIndex());
        WriteFont headWriteFont = new WriteFont();
        headWriteFont.setBold(true);
        headWriteFont.setFontName("宋体");
        headWriteFont.setFontHeightInPoints((short) 12);
        headWriteCellStyle.setWriteFont(headWriteFont);

        //内容样式策略策略
        WriteCellStyle contentWriteCellStyle = new WriteCellStyle();
        // 设置背景颜色白色
        contentWriteCellStyle.setFillForegroundColor(IndexedColors.WHITE.getIndex());
        // 设置垂直居中为居中对齐
        contentWriteCellStyle.setVerticalAlignment(VerticalAlignment.CENTER);
        // 设置左右对齐为靠左对齐
        contentWriteCellStyle.setHorizontalAlignment(HorizontalAlignment.LEFT);
        // 设置单元格上下左右边框为细边框
        contentWriteCellStyle.setBorderBottom(BorderStyle.MEDIUM);
        contentWriteCellStyle.setBorderLeft(BorderStyle.MEDIUM);
        contentWriteCellStyle.setBorderRight(BorderStyle.MEDIUM);
        contentWriteCellStyle.setBorderTop(BorderStyle.MEDIUM);
        //创建字体对象
        WriteFont contentWriteFont = new WriteFont();
        //内容字体大小
        contentWriteFont.setFontName("宋体");
        contentWriteFont.setFontHeightInPoints((short) 14);
        contentWriteCellStyle.setWriteFont(contentWriteFont);
        // 初始化表格样式
        HorizontalCellStyleStrategy horizontalCellStyleStrategy = new HorizontalCellStyleStrategy(headWriteCellStyle, contentWriteCellStyle);
        return horizontalCellStyleStrategy;
    }
~~~



##### 自定义单元格样式

~~~java
package com.james.easy.excel.demo.utils;

import com.alibaba.excel.metadata.CellData;
import com.alibaba.excel.metadata.Head;
import com.alibaba.excel.util.StyleUtil;
import com.alibaba.excel.write.handler.CellWriteHandler;
import com.alibaba.excel.write.metadata.holder.WriteSheetHolder;
import com.alibaba.excel.write.metadata.holder.WriteTableHolder;
import com.alibaba.excel.write.metadata.style.WriteCellStyle;
import com.alibaba.excel.write.metadata.style.WriteFont;
import org.apache.poi.ss.usermodel.*;

import java.util.HashMap;
import java.util.List;

/**
 * @author james
 * @version 1.0
 * @description: 设置单元格格式
 * @date 2021/10/16 15:46
 */
public class CellColorSheetWriteHandler implements CellWriteHandler {

    /**
     * map
     * key：第i行
     * value：第i行中单元格索引集合
     */
    private HashMap<Integer, List<Integer>> map;

    /**
     * 颜色
     */
    private Short colorIndex;

    /**
     * 有参构造
     */
    public CellColorSheetWriteHandler(HashMap<Integer, List<Integer>> map, Short colorIndex) {
        this.map = map;
        this.colorIndex = colorIndex;
    }

    /**
     * 无参构造
     */
    public CellColorSheetWriteHandler() {

    }

    /**
     * 在创建单元格之前调用
     */
    @Override
    public void beforeCellCreate(WriteSheetHolder writeSheetHolder, WriteTableHolder writeTableHolder, Row row, Head head, Integer columnIndex, Integer relativeRowIndex, Boolean isHead) {
    }

    /**
     * 在单元格创建后调用
     */
    @Override
    public void afterCellCreate(WriteSheetHolder writeSheetHolder, WriteTableHolder writeTableHolder, Cell cell, Head head, Integer relativeRowIndex, Boolean isHead) {
    }

    @Override
    public void afterCellDataConverted(WriteSheetHolder writeSheetHolder, WriteTableHolder writeTableHolder, CellData cellData, Cell cell, Head head, Integer integer, Boolean aBoolean) {

    }

    /**
     * 在单元上的所有操作完成后调用
     */
    @Override
    public void afterCellDispose(WriteSheetHolder writeSheetHolder, WriteTableHolder writeTableHolder, List<CellData> cellDataList, Cell cell, Head head, Integer relativeRowIndex, Boolean isHead) {

        Sheet sheet = writeSheetHolder.getSheet();

        /**
         * 考虑到导出数据量过大的情况，不对每一行的每一个单元格进行样式设置，只设置必要行中的某个单元格的样式
         */
        //当前行的第i列
        int i = cell.getColumnIndex();
        //不处理第一行
        if (0 != cell.getRowIndex()) {
            List<Integer> integers = map.get(cell.getRowIndex());
            if (integers != null && integers.size() > 0) {
                if (integers.contains(i)) {
                    // 根据单元格获取workbook
                    Workbook workbook = cell.getSheet().getWorkbook();
                    //设置行高
                    writeSheetHolder.getSheet().getRow(cell.getRowIndex()).setHeight((short) (1.4 * 256));
                    writeSheetHolder.getSheet().setDefaultColumnWidth(200);
                    // 单元格策略
                    WriteCellStyle contentWriteCellStyle = new WriteCellStyle();
                    // 设置背景颜色白色
                    contentWriteCellStyle.setFillForegroundColor(IndexedColors.WHITE.getIndex());
                    // 设置垂直居中为居中对齐
                    contentWriteCellStyle.setVerticalAlignment(VerticalAlignment.CENTER);
                    // 设置左右对齐为靠左对齐
                    contentWriteCellStyle.setHorizontalAlignment(HorizontalAlignment.LEFT);
                    // 设置单元格上下左右边框为细边框
                    contentWriteCellStyle.setBorderBottom(BorderStyle.MEDIUM);
                    contentWriteCellStyle.setBorderLeft(BorderStyle.MEDIUM);
                    contentWriteCellStyle.setBorderRight(BorderStyle.MEDIUM);
                    contentWriteCellStyle.setBorderTop(BorderStyle.MEDIUM);


                    // 创建字体实例
                    WriteFont cellWriteFont = new WriteFont();
                    // 设置字体大小
                    cellWriteFont.setFontName("宋体");
                    cellWriteFont.setFontHeightInPoints((short) 14);
                    //设置字体颜色
                    cellWriteFont.setColor(colorIndex);
                    //单元格颜色
                    contentWriteCellStyle.setFillForegroundColor(IndexedColors.GREY_25_PERCENT.getIndex());
                    contentWriteCellStyle.setWriteFont(cellWriteFont);

                    CellStyle cellStyle = StyleUtil.buildHeadCellStyle(workbook, contentWriteCellStyle);
                    //设置当前行第i列的样式
                    cell.getRow().getCell(i).setCellStyle(cellStyle);
                }
            }
        }
    }

}

~~~



##### 设置导出列的自适应宽度

~~~java
package com.james.easy.excel.demo.utils;

import com.alibaba.excel.enums.CellDataTypeEnum;
import com.alibaba.excel.metadata.CellData;
import com.alibaba.excel.metadata.Head;
import com.alibaba.excel.util.CollectionUtils;
import com.alibaba.excel.write.metadata.holder.WriteSheetHolder;
import com.alibaba.excel.write.style.column.AbstractColumnWidthStyleStrategy;
import org.apache.poi.ss.usermodel.Cell;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author james
 * @version 1.0
 * @description: 导出列的自适应宽度
 * @date 2021/10/16 17:12
 */

public class CustomCellWriteHandler extends AbstractColumnWidthStyleStrategy {
    private static final int MAX_COLUMN_WIDTH = 255;
    private  Map<Integer, Map<Integer, Integer>> CACHE = new HashMap(8);

    public CustomCellWriteHandler() {
    }

    @Override
    protected void setColumnWidth(WriteSheetHolder writeSheetHolder, List<CellData> cellDataList, Cell cell, Head head, Integer relativeRowIndex, Boolean isHead) {
        boolean needSetWidth = isHead || !CollectionUtils.isEmpty(cellDataList);
        if (needSetWidth) {
            Map<Integer, Integer> maxColumnWidthMap = (Map)CACHE.get(writeSheetHolder.getSheetNo());
            if (maxColumnWidthMap == null) {
                maxColumnWidthMap = new HashMap(16);
                CACHE.put(writeSheetHolder.getSheetNo(), maxColumnWidthMap);
            }

            Integer columnWidth = this.dataLength(cellDataList, cell, isHead);
            if (columnWidth >= 0) {
                if (columnWidth > 255) {
                    columnWidth = 255;
                }

                Integer maxColumnWidth = (Integer)((Map)maxColumnWidthMap).get(cell.getColumnIndex());
                if (maxColumnWidth == null || columnWidth > maxColumnWidth) {
                    ((Map)maxColumnWidthMap).put(cell.getColumnIndex(), columnWidth);
                    writeSheetHolder.getSheet().setColumnWidth(cell.getColumnIndex(), columnWidth * 256);
                }

            }
        }
    }

    private Integer dataLength(List<CellData> cellDataList, Cell cell, Boolean isHead) {
        if (isHead) {
            return cell.getStringCellValue().getBytes().length;
        } else {
            CellData cellData = (CellData)cellDataList.get(0);
            CellDataTypeEnum type = cellData.getType();
            if (type == null) {
                return -1;
            } else {
                switch(type) {
                    case STRING:
                        return cellData.getStringValue().getBytes().length;
                    case BOOLEAN:
                        return cellData.getBooleanValue().toString().getBytes().length;
                    case NUMBER:
                        return cellData.getNumberValue().toString().getBytes().length;
                    default:
                        return -1;
                }
            }
        }
    }
}


~~~



##### 生成excel

~~~java
    /**
     * 生成excel
     * @param excelName excel名称
     * @param sheetName excel sheet名称
     * @param bodyList excel 主数据
     * @param headList excel 头标题
     */
    public void generateExcel(String excelName, String sheetName, List<List<String>> bodyList, List<List<String>> headList) {

        HorizontalCellStyleStrategy horizontalCellStyleStrategy = defaultStyles();

        CustomCellWriteHandler handler = new CustomCellWriteHandler();

        //指定单元格样式
        //用来记录需要为第`key`行中的第`value.get(i)`列设置样式
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        CellColorSheetWriteHandler writeHandler = new CellColorSheetWriteHandler(map, IndexedColors.RED.getIndex());
        EasyExcel.write(excelName + ".xlsx").excelType(ExcelTypeEnum.XLSX).sheet().sheetName(sheetName)
                .head(headList).automaticMergeHead(true)
                .registerWriteHandler(horizontalCellStyleStrategy)
                .registerWriteHandler(writeHandler)
                .registerWriteHandler(handler)
                .doWrite(bodyList);
    }

~~~



##### 测试

~~~java
    public static void main(String[] args) {
        
        EasyExcelUtil util = new EasyExcelUtil();
        // excel 表数据
        List<List<String>> bodyList = new ArrayList<>();
        // excel 表头
        List<List<String>> headList = new ArrayList<>();
        String[] headerName = {"test1", "test2", "test3", "test4", "test5", "test6", "test7", "test8"};
        for (int i = 0; i < headerName.length; i++) {
            List<String> list = new ArrayList<>();
            list.add(headerName[i]);
            headList.add(list);
        }
        
        List<String> list = new ArrayList<>();
        List<String> list1 = new ArrayList<>();
        List<String> list2 = new ArrayList<>();
        list.add("1");
        list.add("2");
        list.add("3");
        list.add("4");

        list1.add("5");
        list1.add("6");
        list1.add("7");
        list1.add("8");

        list2.add("1");
        list2.add("9");
        list2.add("30");
        list2.add("40");

        bodyList.add(list1);
        bodyList.add(list);
        bodyList.add(list2);
        
        util.generateExcel(DateUtil.now(), "sheet", bodyList, headList);
    }
~~~