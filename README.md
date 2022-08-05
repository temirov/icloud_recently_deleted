# Remove recently deleted iCloud Drive files

## Context

When you store files on iCloud Drive their deletion is delayed. After the deletion the files are placed in a hard to access Recently Deleted folder where they stay for 30 days before the final purge. This is typically a good thing.

But when the storage is full all other Apple iCloud services, be it Mail or Notes, will stop working. There are two obvious choices left: clear up the iCloud Drive and wait for 30 days *or* pay for the storage upgrade. 

The third option is to delete the files from the recently deleted folder. Alas, there is no access to it but from the web interface. And there is a bug when deleting more than a thousand files which prevents delete all files in case there is more than a 1000.

![](icloud%20bug.png)

## Solution

Here is a very crude solution that can help in case you need to restore access to your Apple services without paying for upgrade or waiting for the 30 days after the deletion.
 
1. Log into icloud.com. Go to the account settings. Click on recent files. Wait for the screen to load.

![](icloud%20recently%20deleted.png)
 
2. Run the script
    1. Open Development tools of the browser
    1. Choose the iframe for the execution context.
    1. Copy-paste the following snippet into the console.

Open developers tools. Go to sources, snippets and add the following snippet.
```javascript
function deleteDocs() {
    document
        .querySelectorAll(".cw-ui-checkbox:not(.select-all-docs) input[type=checkbox]")
        .forEach(
            function(el){el.click();}
        );
    document
        .getElementsByClassName("purge-action-button")[0]
        .click();
    document
        .getElementsByClassName("destructive")[0]
        .click();
}

const filesToDelete = 5000;
const batchSize = 3;
const timeOutInMs = 2000;

# Change script execution context
const frame = document.getElementById("frame1").contentWindow;
cd(frame);

for (let index = 0; index < (filesToDelete / batchSize); index++) {
    setTimeout(deleteDocs, timeOutInMs * index);
}
```

Save and execute the script.

## No warranties
 
Please, note that class identifiers may change in the future and the script may need to be updated. In case you find such discrepancies, please, don't hesitate to submit a fixing PR.
