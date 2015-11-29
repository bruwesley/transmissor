Imports System
Imports System.IO
Imports System.Net
Imports System.Text
Imports System.Web
Imports System.Threading
Imports System.ComponentModel
Imports System.Text.RegularExpressions



Public Class Form1
    Private WithEvents backgroundWorker1 As BackgroundWorker
    Public WithEvents serialPort1 As New IO.Ports.SerialPort

    Public readBuffer As String


    Private thrConnect As Thread

    Dim ConnectResult As String




    ' This method is called by BackgroundWorker's  
    ' RunWorkerCompleted event.  Because it runs in the 
    ' main thread, it can safely set textBox1.Text. 
    Private Sub backgroundWorker1_RunWorkerCompleted( _
        ByVal sender As Object, _
        ByVal e As RunWorkerCompletedEventArgs) _
        Handles backgroundWorker1.RunWorkerCompleted

        TextBox1.Text = ConnectResult
    End Sub




    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        serialPortOpen(3)

    End Sub

    Private Sub SerialPort1_DataReceived(ByVal sender As System.Object, _
                                     ByVal e As System.IO.Ports.SerialDataReceivedEventArgs) _
                                     Handles serialPort1.DataReceived

        Try
            readBuffer = serialPort1.ReadExisting() '.ReadLine()
            'data to UI thread 
            Me.Invoke(New EventHandler(AddressOf DoUpdate))
        Catch ex As Exception
            MsgBox("read " & ex.Message)
        End Try

    End Sub


    Public Sub DoUpdate(ByVal sender As Object, ByVal e As System.EventArgs)
        TextBox1.Text = readBuffer

        Dim TAGreceived As String
        Dim startPos As Integer

        ' startPos = InStr(readBuffer, "]")
        ' If startPos > 0 Then
        TAGreceived = Trim(Mid(readBuffer, startPos + 1, readBuffer.Length - startPos + 1))

        Dim arrParts = Split(readBuffer, " = ")


        Dim valor = arrParts(arrParts.Length - 1).Replace(Chr(13) + Chr(10), "")

        'Dim p as New ProcessStartInfo(@"command", args)
 

        'C:\ZabixAgent\bin\win32\zabbix_sender -z 192.168.56.101 -p 10051 -k "corr" -o "57" -s "Arduino"
        'Process.Start("C:\ZabixAgent\bin\win32\zabbix_sender.exe", "-z ec2-52-23-159-248.compute-1.amazonaws.com -p 10051 -k ""corr"" -o """ + valor + """ -s ""Arduino""")
        Dim p As New ProcessStartInfo("C:\ZabixAgent\bin\win32\zabbix_sender.exe", "-z ec2-52-23-159-248.compute-1.amazonaws.com -p 10051 -k ""corr"" -o """ + valor + """ -s ""Arduino""")

        p.WindowStyle = ProcessWindowStyle.Hidden
        p.CreateNoWindow = True
        Process.Start(p)

        'Process.Start("cmd", "/c C:\ZabixAgent\bin\win32\zabbix_sender -z 192.168.56.101 -p 10051 -k "corr" -o "57" -s "Arduino'")

        'End If

        'Add("C:\Users\ERICONETTO\Documents\Hacka\Massa\temp1.txt", valor)

        Thread.Sleep(1000)
    End Sub



    Public Sub serialPortOpen(ByVal COM_PortNumber As Integer)

        'Configure and open the serial port


        'If the port is already open, close it first
        If serialPort1.IsOpen Then
            TextBox1.Text = "COM" & COM_PortNumber & " estava aberta, fechando para tentar conectar..."
            serialPort1.Close()
        End If

        TextBox1.Text = "Tentando conectar na COM" & COM_PortNumber & "..."
        Try
            With serialPort1
                .PortName = "com" & COM_PortNumber
                .BaudRate = 250000
                .Encoding = System.Text.Encoding.ASCII
                .NewLine = Chr(13) + Chr(10)
            End With

            'Open the port and clear any junck in the input buffer
            serialPort1.Open()
            serialPort1.DiscardInBuffer()
            TextBox1.Text = "Conectado na COM" & COM_PortNumber & "."

        Catch Ex As Exception
            'Handle any exceptions here
            TextBox1.Text = "Falha ao se conectar com a COM" & COM_PortNumber & " '" & Ex.Message & "'"
        End Try
    End Sub



    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click
        If TextBox2.Text = "" Then
            MsgBox("Preencha o número da porta!")
            Exit Sub
        End If
        If IsNumeric(TextBox2.Text) = False Then
            MsgBox("Preencha com números!")
            Exit Sub
        End If

        serialPortOpen(TextBox2.Text)
    End Sub


















End Class
