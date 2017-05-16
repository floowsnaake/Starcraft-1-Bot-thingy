;http://prntscr.com/f8jl5n
 
;floowsnaake
;starcraft 1.18.5.1581 - bot thingy :P
; https://github.com/floowsnaake


IF NOT A_IsAdmin
{
   Run *RunAs "%A_ScriptFullPath%"
   ExitApp
}
setSeDebugPrivilege(enable := True)

#SingleInstance Force

ProcessName := "StarCraft.exe"
hwnd := MemoryOpenFromName(ProcessName)


CoordMode, Mouse, Client
CoordMode, Pixel, Client
CoordMode, ToolTip, Client

Minerals = 0x00CBA088
Units = 0x00CBA049
MaxUnits = 0x00CBA04C

Gui, GUI_Overlay:New, +AlwaysOnTop +hwndGUI_Overlay_hwnd
    ;Gui, Margin, 10, 10
    Gui, Font, s10 q4, Segoe UI Bold
    Gui, Add, Text, w200  vTEXT_Timer cBlue,
    Gui, Add, Text, w200  vTEXT_Timer2 cBlue, 
Gui, Add, Text, w200  vTEXT_Timer3 cBlue, 

    Gui, Color, 000000
    WinSet, Transparent, 220
    Winset, AlwaysOnTop, on
    Gui, Show, Hide, Overlay
 
  
    Gui, GUI_Overlay:Show, NoActivate, FLK
  

SetTimer,UpdateMemory,1000

Sleep, 200
 Home:   
Sleep, 200
return

UpdateMemory:
Minerals_GUI = % MemoryRead(hwnd, Minerals, "str",4)
Units_GUI = % MemoryRead(hwnd, Units, "str",3)
MaxUnits_GUI = % MemoryRead(hwnd, MaxUnits, "str",3)

GuiControl, GUI_Overlay:, TEXT_Timer,Minerals: %Minerals_GUI%
GuiControl, GUI_Overlay:, TEXT_Timer2,Units: %Units_GUI%
GuiControl, GUI_Overlay:, TEXT_Timer3,MaxUnits: %MaxUnits_GUI%
return

; ==================================================
; FUCNTIONS!
; ==================================================


MemoryOpenFromName(Name)
{
    Process, Exist, %Name%
    Return DllCall("OpenProcess", "Uint", 0x1F0FFF, "int", 0, "int", PID := ErrorLevel)
}

MemoryWrite(hwnd, address, writevalue, datatype="int", length=4, offset=0)
{
	VarSetCapacity(finalvalue, length, 0)
	NumPut(writevalue, finalvalue, 0, datatype)
	return DllCall("WriteProcessMemory", "Uint", hwnd, "Uint", address+offset, "Uint", &finalvalue, "Uint", length, "Uint", 0)
}

 ; UInt, Int, Int64, Short, UShort, Char, UChar, Double, Float, Ptr or UPtr
MemoryRead(hwnd, address, datatype="int", length=4, offset=0)
{
	VarSetCapacity(readvalue,length, 0)
	DllCall("ReadProcessMemory","Uint",hwnd,"Uint",address+offset,"str",readvalue,"Uint",length,"Uint *",0)

if (datatype = "Float")
finalvalue := NumGet(readvalue, 0, "Float")
if (datatype = "Str")
finalvalue := StrGet(&readvalue, length, "UTF-8")
if (datatype = "StrUni")
finalvalue := StrGet(&readvalue, length, "UTF-16")
if NOT (datatype = "Float" or datatype = "Str" or datatype = "StrUni")
{
finalvalue := NumGet(readvalue, 0, datatype)
}
return finalvalue
}

setSeDebugPrivilege(enable := True)
{
    h := DllCall("OpenProcess", "UInt", 0x0400, "Int", false, "UInt", DllCall("GetCurrentProcessId"), "Ptr")
    ; Open an adjustable access token with this process (TOKEN_ADJUST_PRIVILEGES = 32)
    DllCall("Advapi32.dll\OpenProcessToken", "Ptr", h, "UInt", 32, "PtrP", t)
    VarSetCapacity(ti, 16, 0)  ; structure of privileges
    NumPut(1, ti, 0, "UInt")  ; one entry in the privileges array...
    ; Retrieves the locally unique identifier of the debug privilege:
    DllCall("Advapi32.dll\LookupPrivilegeValue", "Ptr", 0, "Str", "SeDebugPrivilege", "Int64P", luid)
    NumPut(luid, ti, 4, "Int64")
    if enable
    	NumPut(2, ti, 12, "UInt")  ; enable this privilege: SE_PRIVILEGE_ENABLED = 2
    ; Update the privileges of this process with the new access token:
    r := DllCall("Advapi32.dll\AdjustTokenPrivileges", "Ptr", t, "Int", false, "Ptr", &ti, "UInt", 0, "Ptr", 0, "Ptr", 0)
    DllCall("CloseHandle", "Ptr", t)  ; close this access token handle to save memory
    DllCall("CloseHandle", "Ptr", h)  ; close this process handle to save memory
    return r
}

^Esc::
ExitApp
return

