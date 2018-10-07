---
layout: post
title: "Automating Excel Reports with Python"
date: 2018-04-29
---

guys - this is life-changing. AUTOMATE ALL YOUR EXCEL REPORTS WITH PYTHON!!

__1) Basic writing of dataframe from pandas into an excel sheet__ see here: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_excel.html AND http://xlsxwriter.readthedocs.io/example_pandas_multiple.html

```python

# writing one dataframe to one excel file
df.to_excel()


# writing multiple dataframas into different sheets
writer = pd.ExcelWriter('pandas_multiple.xlsx', engine='xlsxwriter')

df1.to_excel(writer, sheet_name='Sheet1')
df2.to_excel(writer, sheet_name='Sheet2')
df3.to_excel(writer, sheet_name='Sheet3')

writer.save()
```

__2) Write dataframe from pandas into excel sheet with number and cell color formatting__:

```python

# write to excel   
writer = pd.ExcelWriter(destination_filepath,engine='xlsxwriter')   
workbook=writer.book
worksheet=workbook.add_worksheet('sheetname')
writer.sheets['sheetname'] = worksheet

# define formats
format_num = workbook.add_format({'num_format': '_(* #,##0_);_(* (#,##0);_(* "-"??_);_(@_)'})
format_perc = workbook.add_format({'num_format': '0%;-0%;"-"'})
format_header = workbook.add_format({'bold': True, 'bg_color': '#C6EFCE'})

# write individual cells
worksheet.write(0, 2, "sheetname", format_header)
worksheet.write(1, 2, sum(df['abc']))
worksheet.write(1, 3, sum(df['def']))
worksheet.write(1, 4, sum(df['ghi']))

# write dataframe in
df.to_excel(writer,sheet_name='sheetname', startrow=2, startcol=0, index=False)  
worksheet.set_column(1, 1, 45)
worksheet.set_column(2, 4, 15, format_num)
worksheet.set_column(5, 5, 15, format_perc)
writer.save

```

__3) Write your dataframe into pre-formatted Excel sheets__ see here: https://stackoverflow.com/questions/9920935/easily-write-formatted-excel-from-python-start-with-excel-formatted-use-it-in

```python

import xlrd 
import xlutils.copy 

inBook = xlrd.open_workbook('input.xls', formatting_info=True) 
outBook = xlutils.copy.copy(inBook) 

def _getOutCell(outSheet, colIndex, rowIndex): 
    """ HACK: Extract the internal xlwt cell representation. """ 
    row = outSheet._Worksheet__rows.get(rowIndex) 
    if not row: return None 
    cell = row._Row__cells.get(colIndex) 
    return cell 

def setOutCell(outSheet, col, row, value): 
    """ Change cell value without changing formatting. """ 
    # HACK to retain cell style. 
    previousCell = _getOutCell(outSheet, col, row) 
    # END HACK, PART I 
    outSheet.write(row, col, value) 
    # HACK, PART II 
    if previousCell: 
        newCell = _getOutCell(outSheet, col, row) 
    if newCell: 
        newCell.xf_idx = previousCell.xf_idx 
    # END HACK 


outSheet = outBook.get_sheet(0) 
setOutCell(outSheet, 5, 5, 'Test') 
outBook.save('output.xls') 

```


__3) Creating a PivotTable in Excel__ see here: https://stackoverflow.com/questions/22532019/creating-pivot-table-in-excel-using-python

```python

import win32com.client
Excel   = win32com.client.gencache.EnsureDispatch('Excel.Application') # Excel = win32com.client.Dispatch('Excel.Application')

win32c = win32com.client.constants

wb = Excel.Workbooks.Add()
Sheet1 = wb.Worksheets("Sheet1")

TestData = [['Country','Name','Gender','Sign','Amount'],
             ['CH','Max' ,'M','Plus',123.4567],
             ['CH','Max' ,'M','Minus',-23.4567],
             ['CH','Max' ,'M','Plus',12.2314],
             ['CH','Max' ,'M','Minus',-2.2314],
             ['CH','Sam' ,'M','Plus',453.7685],
             ['CH','Sam' ,'M','Minus',-53.7685],
             ['CH','Sara','F','Plus',777.666],
             ['CH','Sara','F','Minus',-77.666],
             ['DE','Hans','M','Plus',345.088],
             ['DE','Hans','M','Minus',-45.088],
             ['DE','Paul','M','Plus',222.455],
             ['DE','Paul','M','Minus',-22.455]]

for i, TestDataRow in enumerate(TestData):
    for j, TestDataItem in enumerate(TestDataRow):
        Sheet1.Cells(i+2,j+4).Value = TestDataItem

cl1 = Sheet1.Cells(2,4)
cl2 = Sheet1.Cells(2+len(TestData)-1,4+len(TestData[0])-1)
PivotSourceRange = Sheet1.Range(cl1,cl2)

PivotSourceRange.Select()

Sheet2 = wb.Worksheets(2)
cl3=Sheet2.Cells(4,1)
PivotTargetRange=  Sheet2.Range(cl3,cl3)
PivotTableName = 'ReportPivotTable'

PivotCache = wb.PivotCaches().Create(SourceType=win32c.xlDatabase, SourceData=PivotSourceRange, Version=win32c.xlPivotTableVersion14)

PivotTable = PivotCache.CreatePivotTable(TableDestination=PivotTargetRange, TableName=PivotTableName, DefaultVersion=win32c.xlPivotTableVersion14)

PivotTable.PivotFields('Name').Orientation = win32c.xlRowField
PivotTable.PivotFields('Name').Position = 1
PivotTable.PivotFields('Gender').Orientation = win32c.xlPageField
PivotTable.PivotFields('Gender').Position = 1
PivotTable.PivotFields('Gender').CurrentPage = 'M'
PivotTable.PivotFields('Country').Orientation = win32c.xlColumnField
PivotTable.PivotFields('Country').Position = 1
PivotTable.PivotFields('Country').Subtotals = [False, False, False, False, False, False, False, False, False, False, False, False]
PivotTable.PivotFields('Sign').Orientation = win32c.xlColumnField
PivotTable.PivotFields('Sign').Position = 2

DataField = PivotTable.AddDataField(PivotTable.PivotFields('Amount'))
DataField.NumberFormat = '#\'##0.00'

Excel.Visible = 1

wb.SaveAs('ranges_and_offsets.xlsx')
Excel.Application.Quit()

```


__Special feature for xlwings__ see here: http://docs.xlwings.org/en/stable/quickstart.html


---

__Update (2018-10-08)__:

So I've been approached to share this link here: https://www.pyxll.com/blog/tools-for-working-with-excel-and-python/

Mostly a pitch for PyXLL, but also includes snippets of various tools for working with python and excel.

Always helpful to know about alternative solutions out there and their pros and cons (lots of features and seems fairly powerful - if you can convince your company to fork out $250 a year, per user).

For me... I think I will stick to the free and open source alternatives for now :p