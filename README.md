# harvest-otrs-integration

This integration between OTRS ver.3.2.11 (https://www.otrs.com/) and Harvest (https://www.getharvest.com/) applications is created using Harvest Widget (https://github.com/harvesthq/platform/blob/master/widget.md)

Other possible integration options are described here: https://www.getharvest.com/add-time-tracking

###Step 1. Update AgentTickerZoom.dtl
In Windows OS the file AgentTickerZoom.dtl is located by default in C:\otrs\OTRS\Kernel\Output\HTML\Standard folder
Open the file and do these changes:

Replace code:
```html
<!-- dtl:block:Owner -->
                        <label>$Text{"Owner"}:</label>
                        <p class="Value" title="$QData{"UserFirstname"} $QData{"UserLastname"} ($QData{"UserLogin"})">$Quote{"$Data{"UserFirstname"} $Data{"UserLastname"}","18"}</p>
                        <div class="Clear"><br/></div>
<!-- dtl:block:Owner -->
```

with this one:
```html
<!-- dtl:block:Owner -->
                        <label>$Text{"Owner"}:</label>
                        <p class="Value" title="$QData{"UserFirstname"} $QData{"UserLastname"} ($QData{"UserLogin"})">$Quote{"$Data{"UserFirstname"} $Data{"UserLastname"}","18"}</p>
                        <div class="Clear"><br/></div>
<div id="harvest" class="wrap">
<iframe id="harvest_frame" class="frame"
  src="https://platform.harvestapp.com/platform/timer?app_name=MYCOMPANY&closable=false&permalink=http%3A%2F%LOCALOTRSHOST%3A801%2Fotrs%2Findex.pl%3FAction%3DAgentTicketZoom%3BTicketID%3D$QData{"TicketID"}&external_item_id=$QData{"TicketID"}&external_item_name=Ticket%23$Data{"TicketNumber"} $LQData{"Title"}">
</iframe>
</div>
<!-- dtl:block:Owner -->
```
where MYCOMPANY is the registered company name in Harvest application.
LOCALOTRSHOST is the hostname of your local OTRS system

You should use URL encoding to replace all special characters in the code. Here is the free tool that lets you encode and decode your URL string: http://meyerweb.com/eric/tools/dencoder/

###Step 2. Update HTMLHead.dtl
In Windows OS the file HTMLHead.dtl is located by default in C:\otrs\OTRS\Kernel\Output\HTML\Standard folder
Open the file and do these changes:

Replace code:
```html
<!-- dtl:block:HeaderLogoCSS -->
    <style type="text/css">
        #Header #Logo {
            background-image: $QData{"URL"};
            top: $QData{"StyleTop"};
            right: $QData{"StyleRight"};
            width: $QData{"StyleWidth"};
            height: $QData{"StyleHeight"};
        }
    </style>
  <!-- dtl:block:HeaderLogoCSS -->
  ```

with this code:
```html
<!-- dtl:block:HeaderLogoCSS -->
    <style type="text/css">
        #Header #Logo {
            background-image: $QData{"URL"};
            top: $QData{"StyleTop"};
            right: $QData{"StyleRight"};
            width: $QData{"StyleWidth"};
            height: $QData{"StyleHeight"};
        }
		
.wrap
{
    width: 250px;
    padding: 0;
    overflow: hidden;
}

.frame
{
    width: 330px;
    border: 0;

    -ms-transform: scale(0.75);
    -moz-transform: scale(0.75);
    -o-transform: scale(0.75);
    -webkit-transform: scale(0.75);
    transform: scale(0.75);

    -ms-transform-origin: 0 0;
    -moz-transform-origin: 0 0;
    -o-transform-origin: 0 0;
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
}
    </style>
	
<script type="text/javascript">
window.addEventListener("message", function (event) {
  if (event.origin != "https://platform.harvestapp.com") {
    return;
  }

  if (event.data.type == "frame:resize") {
    document.querySelector("#harvest").style.height = event.data.value*0.75 + "px";
	document.querySelector("#harvest_frame").style.height = event.data.value + "px";
  }
});
</script>
	
<!-- dtl:block:HeaderLogoCSS -->
```


That's it! Now open any ticket in OTRS web site and you should see Harvest Widget on the right site of your ticket window.

Cheers

