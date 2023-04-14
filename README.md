# VBA-challenge
Sub Stock_Script():

'Declare Variables
        Dim Open_Price As Double
        Dim Close_Price As Double
        Dim Yearly_Change As Double
        Dim Percent_Change As Double
        Dim Volume As Double
        Dim LastRow As Long
        Dim ws As Worksheet
        For Each ws In Worksheets
        
'Insert New Columns
    Cells(1, 9).Value = "Ticker"
    Cells(1, 10).Value = "Yearly_Change"
    Cells(1, 11).Value = "Percent_ Change"
    Cells(1, 12).Value = "Total_Ticker_Volume"
        
'Count number of rows in worksheet
    LastRow = ws.Cells(Rows.Count, 1).End(xlUp).Row

'Set the location as Integer
    Summary_Table_Row = 2

'Set Initial Variable for holding the Ticker Total Volume Per Ticker
    Total_Ticker_Volume = 0
    Start = 2
    
'Loop through column A and locate unique ticker symbols and insert into column I
    For i = 2 To LastRow
    
'If ticker changes then print results
    If Cells(i + 1, 1).Value <> Cells(i, 1).Value Then
    
'Stores results in variables
    Total = Total + Cells(i, 7).Value
    
    If Total = 0 Then
    
    Range("I" & 2 + j).Value = Cells(i, 1).Value
    Range("J" & 2 + j).Value = 0
    Range("K" & 2 + j).Value = "%" & 0
    Range("L" & 2 + j).Value = 0
Else
    'Find first non zero value
   If Cells(Start, 3) = 0 Then
   For find_value = Start To i
                        If Cells(find_value, 3).Value <> 0 Then
                            Start = find_value
                            Exit For
                        End If
                     Next find_value
                End If

                ' Calculate Change
                Change = (Cells(i, 6) - Cells(Start, 3))
                percentChange = Change / Cells(Start, 3)

                ' start of the next stock ticker
                Start = i + 1

                ' print the results
                Range("I" & 2 + j).Value = Cells(i, 1).Value
                Range("J" & 2 + j).Value = Change
                Range("J" & 2 + j).NumberFormat = "0.00"
                Range("K" & 2 + j).Value = percentChange
                Range("K" & 2 + j).NumberFormat = "0.00%"
                Range("L" & 2 + j).Value = Total

                ' colors positives green and negatives red
                Select Case Change
                    Case Is > 0
                        Range("J" & 2 + j).Interior.ColorIndex = 4
                    Case Is < 0
                        Range("J" & 2 + j).Interior.ColorIndex = 3
                    Case Else
                        Range("J" & 2 + j).Interior.ColorIndex = 0
                End Select

            End If

            ' reset variables for new stock ticker
            Total = 0
            Change = 0
            j = j + 1
            Days = 0

        ' If ticker is still the same add results
        Else
            Total = Total + Cells(i, 7).Value

        End If

    Next i
    
'Conditional Formatting to change color of Yearly_Change Column
    For i = 2 To LastRow
    If Range("J" & i + 1).Value > 0 Then
        Cells(i, "J").Interior.ColorIndex = 4 'Green
        Else
        Cells(i, "J").Interior.ColorIndex = 3 'Red
        End If

    Next i

' Set Greatest % Increase, % Decrease, and Total Volume
        Cells(2, Column + 14).Value = "Greatest % Increase"
        Cells(3, Column + 14).Value = "Greatest % Decrease"
        Cells(4, Column + 14).Value = "Greatest Total Volume"
        Cells(1, Column + 15).Value = "Ticker"
        Cells(1, Column + 16).Value = "Value"
    
'Look through each row to find the greatest value and its associate ticket
    For Z = 2 To YCLastRow
                If Cells(Z, Column + 10).Value = Application.WorksheetFunction.Max(ws.Range("K2:K" & YCLastRow)) Then
                Cells(2, Column + 15).Value = Cells(Z, Column + 8).Value
                Cells(2, Column + 16).Value = Cells(Z, Column + 10).Value
                Cells(2, Column + 16).NumberFormat = "0.00%"
            ElseIf Cells(Z, Column + 10).Value = Application.WorksheetFunction.Min(ws.Range("K2:K" & YCLastRow)) Then
                Cells(3, Column + 15).Value = Cells(Z, Column + 8).Value
                Cells(3, Column + 16).Value = Cells(Z, Column + 10).Value
                Cells(3, Column + 16).NumberFormat = "0.00%"
            ElseIf Cells(Z, Column + 11).Value = Application.WorksheetFunction.Max(ws.Range("L2:L" & YCLastRow)) Then
                Cells(4, Column + 15).Value = Cells(Z, Column + 8).Value
                Cells(4, Column + 16).Value = Cells(Z, Column + 11).Value
            End If
            
Next Z
            Next ws
            End Sub
