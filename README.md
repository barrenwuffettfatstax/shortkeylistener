# shortkeylistener
param([string]$inputtxtinsert,[int]$inputtag,[string]$inputflag) 


  
  $timetag = get-date((get-date).AddHours(2)) -Uformat "%m/%d/%Y %I:%M %p"
  $inittag = " - EC, PharmD - FAX "
  $clipstring = "$inputtxtinsert"
  $gabastring = "$inputtxtinsert"
#  Write-Host $inputtxtinsert
#  Write-Host $gabastring

function Get-ClipboardText()
{
    Add-Type -AssemblyName System.Windows.Forms
    $tb = New-Object System.Windows.Forms.TextBox
    $tb.Multiline = $true
    $tb.Paste()
    $tb.Text
}

    $clipstring =  Get-ClipboardText 
 
#  add-type -an system.windows.forms; $clipstring = # [System.Windows.Forms.Clipboard]::GetText();
  add-type -an system.windows.forms; $gabastring = [System.Windows.Forms.Clipboard]::GetText();
  $gabastring = $clipstring.ToString()
  $clipstring = $clipstring.ToLower()
  

  $a,$b,$c,$d = $gabastring.Split(" ")
  [int]$gdose = [convert]::ToInt32($a)
  [int]$gqty = [convert]::ToInt32($b)
  [int]$e = ($gdose*$gqty)/90
  $gabastmt =  "$gdose mg at $b/90 per claims =  $e mg/day $c $d =  = standard allowance"
  Write-Host $gabastmt

  # $clipstring = {add-type -an system.windows.forms; [System.Windows.Forms.Clipboard]::GetText() }
  # Write-Host "$clipstring" is my name
  # $diagnosis = $clipstring.ToString()

function Get-ClipboardText()
# spawns non-drawn text box to copy clipboard text - faster process than PS # GetText
{
    Add-Type -AssemblyName System.Windows.Forms
    $tb = New-Object System.Windows.Forms.TextBox
    $tb.Multiline = $true
    $tb.Paste()
    $tb.Text
}
  
  switch ($inputtag) {
  
    0 {$stampline =  $gabastmt + "$inittag" + "$timetag"}
    1 {$stampline = ($inputtxtinsert).ToString() + "$inittag" + "$timetag"}
    2 {$stampline = $clipstring} 
    5 {$stampline = "the diagnosis of " + $clipstring + " is considered an experimental or investigational use for this drug. Experimental or investigational indications are those which further studies or clinical trials are necessary to determine the maximum tolerated dose, toxicity, safety, or efficacy as compared with the standard means of treatment. For more information, please refer to the 2018 Blue Cross and Blue Shield Service Benefit Plan brochure (RI 71-005). Details regarding experimental or investigational services are listed on page 149"}
    6 {$stampline = "the diagnosis of " + $clipstring + " does not establish medical necessity for this drug. Medical necessity is determined by adherence to generally accepted standards of medical practice in the United States, is clinically appropriate, in terms of type, frequency, extent, site, duration and considered effective for the patient´s illness, injury, disease, or its symptoms. For more information, please refer to the 2018 Blue Cross and Blue Shield Service Benefit Plan brochure (RI 71-005). Details regarding medical necessity are listed on page 150"}
    7 {$stampline = "no/not " + $clipstring +"$inittag" + "$timetag"}
    8 {$stampline = "unknown " + $clipstring +"$inittag" + "$timetag"}
    9 {$stampline = "yes " + $clipstring +"$inittag" + "$timetag"}
    10 {$stampline = ($clipstring).ToUpper()}
    12 {$stampline = ($inputtxtinsert).ToString()}
    13 {$stampline = " the quantity requested is greater than the amount generally approved by the plan and therefore medical necessity has not been established for this drug. Medical necessity is determined by adherence to generally accepted standards of medical practice in the United States, is clinically appropriate, in terms of type, frequency, extent, site, duration and considered effective for the patient´s illness, injury, disease, or its symptoms. For more information, please refer to the 2018 Blue Cross and Blue Shield Service Benefit Plan brochure (RI 71-005). Details regarding medical necessity are listed on page 150"}
    default {$stampline = ($inputtxtinsert).ToString()} 
   }
  add-type -an system.windows.forms; [System.Windows.Forms.Clipboard]::SetText($stampline)
  # Write-Output '|' $stampline >> usedshorties.txt
