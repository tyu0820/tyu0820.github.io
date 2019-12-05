---
title: POI生成下拉列表的数量限制问题
categories: POI
tags: POI
---

POI导出excel时，使用以下方法下拉框内容超过255会报错

	ExcelUtils.addDataValidationXls(sheet, roomTypeArray, "请选择下拉框中的内容填写", j+2, j+2, 3, 3);

解决方法：创建一个隐藏的sheet页，把下拉框的内容放到这个sheet页中，然后用公式引用

	HSSFSheet hidden = wb.createSheet("hidden"); 
	int count = 0;
	if(CollectionUtils.isNotEmpty(typeList)){
		count = typeList.size();
		for(int t=0;t<typeList.size();t++){
			TypeDTO dto=typeList.get(t);
			HSSFRow hrow = hidden.createRow(t); 
			HSSFCell hcell = hrow.createCell(0); 
			hcell.setCellValue(dto.getTypeName());
		}
	}
	wb.setSheetHidden(wb.getSheetIndex("hidden"), true);
	Name namedCell=null;
    namedCell = wb.createName(); 
	namedCell.setNameName("hidden");
	String n="hidden";
	namedCell.setRefersToFormula(n+"!$A$1:$A$" + roomCount); 
	DVConstraint constraint = DVConstraint.createFormulaListConstraint("hidden");

	int totalRowNum = 1000;
	String sheetName="测试";
	CellRangeAddressList addressList = new CellRangeAddressList(1, totalRowNum, 3, 3);
	HSSFDataValidation validation = new HSSFDataValidation(addressList, constraint); 
	HSSFSheet sheet = wb.getSheet(sheetName);
	sheet.addValidationData(validation);
	wb.setSheetHidden(0, true); 