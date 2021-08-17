# Documentation on Uima socket
websocket based...

# Github pages
The Project uses Github pages as a free web host it is reachable under [MobileAnnotator](https://cr-heidemann.github.io/MobileAnnotator)
Github pages only supports https. And modern Web Browser only allow secure content if the website is https.
Shapenet and uima both don't support https/wss we thus use a nginx proxy server which reroutes them

In the nginx config in a server unit which supports https use the following:

```nginx
server{
	...
    location /uima/ {

        proxy_pass http://textannotator.texttechnologylab.org/uima;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 1d;

    }

    location /shapenet/ {
    	keepalive_timeout  400;
        proxy_pass http://shapenet.texttechnologylab.org/;
        proxy_set_header X-Forwarded-Proto https;
    }
	...
}
```

In `src/url.config.ts` you can change this secure url which will be necessary if you want to continue using github pages, since nginx runs on a private server which i will not continue running indefinitely, better yes i would be if uima supports wss and shapenet https.

# Documentation on the Mobile Annotator Iso space Tool

**sem-af.Component**
summary: handles how the sem-af-tool works, saves the annotation and link data into data structures; 
handles the colouration and saving the latest annotation

--genearteToolbarMenu()
	generates the toolbar with content unique for the sem-af.Component. While the Buttons that are in the Toolbar are for every Component, the menu is flexible. 
	We added an option to change the font size which is responsible for the token size and one for showing all the links
	The icons are available under
	https://www.angularjswiki.com/de/angular/angular-material-icons-list-mat-icon-list/

**sem-af-picker.Component**
summary: handles the menu that opens for annotation. it's setup as a simple 2 tab menu, which corresponds with the attributes of each entity

**sem-af-link-picker.Component**
summary: handles the menu that opens when link annotation is selected in the sem-af-picker menu. it corresponds with the link attributes

**shapenet-picker.Component**
summary: handles the menu that opens when shape net objects are to be selected. it corresponds with the shapenet id database

-- set-shapenet(id:string) 
	this function saves the id to the data in the sem-af.Component and this component
	
**tool-bar.Component**
summary: changes in this menu allowed for including a "help" button to display the menu

 -- openmanual()
	this functions sole purpose is to open the manual.pdf that is saved for the certain tool
	other manuals for the other tools can be added for the simple if-statement. as there is only one manual now, the help button is hidden for the other tools per the picker.component.html.
	If other manuals are to be added, one can remove the ngIF "title...." that now checks if the title is the one of the Sem-AF annotator, otherwise it won't show, and only keep the button with the function.
 

**contentholderSemAF.Component**
summary: its made from copying the original contentholder, changes allow to visualise links and give more space between tokens for better selections

-- .entryDiv (in contentholder.Component.html)
	border-radius: set to be more rectangular than in the other tools, so that the links can be visualised easier (higher percent - more round; lower percent - more rectangulat)
	margin: needs to be changed to get more space between tokens and between lines