# temp

const AdmZip = require('adm-zip');
const xml2js = require('xml2js');

// Load the .zip file
const zip = new AdmZip('path/to/your/archive.zip');

// Get the content of the XML file you want to edit
const xmlEntry = zip.getEntry('path/to/your/xml/file.xml');

if (xmlEntry) {
    // Read the content of the XML file
    const xmlContent = xmlEntry.getData().toString('utf8');

    // Parse XML content
    xml2js.parseString(xmlContent, (err, result) => {
        if (err) {
            console.error('Error parsing XML:', err);
        } else {
            // Modify the XML data as needed
            // For example, change some value
            result.root.person[0].name = ['Jane'];

            // Convert modified XML data back to XML string
            const builder = new xml2js.Builder();
            const modifiedXml = builder.buildObject(result);

            // Update the content of the XML file within the .zip archive
            zip.updateFile(xmlEntry.entryName, Buffer.from(modifiedXml, 'utf8'));

            // Save the modified .zip file
            zip.writeZip('path/to/your/new/archive.zip', err => {
                if (err) {
                    console.error('Error writing zip file:', err);
                } else {
                    console.log('Zip file has been modified and saved successfully.');
                }
            });
        }
    });
} else {
    console.error('XML file not found in the zip archive.');
}
