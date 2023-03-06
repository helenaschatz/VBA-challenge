### 
Sub MultipleYearStockData():

    For Each ws In Worksheets
    
        Dim WorksheetName As String
        Dim i As Long
        Dim j As Long
        Dim TickCount As Long
        Dim LastRowA As Long
        Dim LastRowI As Long
        Dim PercentChange As Double
        Dim GreatIncrease As Double
        Dim GreatDecrease As Double
        Dim GreatTotalVol As Double
        
     
        WorksheetName = ws.Name
        
        
        ws.Cells(1, 9).Value = "Ticker"
        ws.Cells(1, 10).Value = "Yearly Change"
        ws.Cells(1, 11).Value = "Percent Change"
        ws.Cells(1, 12).Value = "Total Stock Volume"
        ws.Cells(1, 16).Value = "Ticker"
        ws.Cells(1, 17).Value = "Value"
        ws.Cells(2, 15).Value = "Greatest % Increase"
        ws.Cells(3, 15).Value = "Greatest % Decrease"
        ws.Cells(4, 15).Value = "Greatest Total Volume"
        
        'Ticker counter to first row
        TickCount = 2
        
        'start row to 2
        j = 2
        
        'find the last no-blank cell in column A
        LastRowA = ws.Cells(Rows.Count, 1).End(xlUp).Row
       
           
            For i = 2 To LastRowA
            
               
                If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                
                
                ws.Cells(TickCount, 9).Value = ws.Cells(i, 1).Value
                
              
                ws.Cells(TickCount, 10).Value = ws.Cells(i, 6).Value - ws.Cells(j, 3).Value
                
                    'Conditional formating
                    If ws.Cells(TickCount, 10).Value < 0 Then
                
                  
                    ws.Cells(TickCount, 10).Interior.ColorIndex = 3
                
                    Else
                    
                    ws.Cells(TickCount, 10).Interior.ColorIndex = 4
                
                    End If
                    
                    ' percent change
                    If ws.Cells(j, 3).Value <> 0 Then
                    PercentChange = ((ws.Cells(i, 6).Value - ws.Cells(j, 3).Value) / ws.Cells(j, 3).Value)
                    
                    'percent formating
                    ws.Cells(TickCount, 11).Value = Format(PercentChange, "Percent")
                    
                    Else
                    
                    ws.Cells(TickCount, 11).Value = Format(0, "Percent")
                    
                    End If
                    
                'total volume
                ws.Cells(TickCount, 12).Value = WorksheetFunction.Sum(Range(ws.Cells(j, 7), ws.Cells(i, 7)))
                
                'Increase TickCount by 1
                TickCount = TickCount + 1
                
                'new start row of the ticker block
                j = i + 1
                
                End If
            
            Next i
            
        'Find last noblank cell
        LastRowI = ws.Cells(Rows.Count, 9).End(xlUp).Row
    
  
        GreatTotalVol = ws.Cells(2, 12).Value
        GreatIncrease = ws.Cells(2, 11).Value
        GreatDecrease = ws.Cells(2, 11).Value
        
            'Loop
            For i = 2 To LastRowI
            
                If ws.Cells(i, 12).Value > GreatTotalVol Then
                GreatTotalVol = ws.Cells(i, 12).Value
                ws.Cells(4, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatTotalVol = GreatTotalVol
                
                End If

                If ws.Cells(i, 11).Value > GreatIncrease Then
                GreatIncrease = ws.Cells(i, 11).Value
                ws.Cells(2, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatIncrease = GreatIncrease
                
                End If

                If ws.Cells(i, 11).Value < GreatDecrease Then
                GreatDecrease = ws.Cells(i, 11).Value
                ws.Cells(3, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatDecrease = GreatDecrease
                
                End If
                
            'results
            ws.Cells(2, 17).Value = Format(GreatIncrease, "Percent")
            ws.Cells(3, 17).Value = Format(GreatDecrease, "Percent")
            ws.Cells(4, 17).Value = Format(GreatTotalVol, "Scientific")
            
            Next i
            
        'Djust column width automatically
        Worksheets(WorksheetName).Columns("A:Z").AutoFit
            
    Next ws
        
End Sub




