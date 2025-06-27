function copyIdentification() {
    try {
        if (checkWinOpen && (IdentDetailsEdit != null && !IdentDetailsEdit.closed)) {
            alert("Please close other windows");
            return;
        }

        if (CheckForSelectedRow(document.getElementsByName('DocumentSet')) == "false") {
            showUserMessage("MSGCOPY", 'MSGJ0120');
            return;
        }

        for (var k = 0; k < document.all.DocumentSet.rows.length; k++) {
            var row_bgcolor = document.all.DocumentSet.rows[k].className;

            if (row_bgcolor === 'rowHighLighted') {
                var docURL = document.all.DocumentSet.rows[k].getAttribute("EntityDocumentBO.url_");

                // Match the selected row's URL with internal array and fetch its index
                for (var i = 0; i < doc_URL.length; i++) {
                    if (doc_URL[i] === docURL) {
                        break;
                    }
                }

                recordIndex = i;

                // Now populate form fields
                document.frm2.strText1.value = srmEscape(ad_ID[i], '^', escChars); // ID
                document.frm2.Type.value = srmEscape(ad_identifierType[i], '^', escChars); // Type
                document.frm2.dtDate2.value = srmEscape(ad_validityDate[i], '^', escChars); // Validity Date
                document.frm2.dtDate1.value = srmEscape(ad_dateOfIssue[i], '^', escChars); // Date of Issue
                document.frm2.strText2.value = srmEscape(ad_placeOfIssue[i], '^', escChars); // Place of Issue
                document.frm2.IDIssuedOrganisation.value = srmEscape(ad_IDIssuedOrg[i], '^', escChars); // Issued Org

                // Tracker# 97236 — Issued Org field handling
                // Add more fields if EntityDocumentBO has more attributes to pre-fill

                // Popup handling
                checkWinOpen = true;
                IdentDetailsEdit = window.open(
                    '../common/html/SSOblank.html',
                    'IdentDetails',
                    'directories=No,height=500,width=700,resizable=no,titlebar=no,toolbar=no,status=no,scrollbars=yes'
                );

                document.frm2.target = 'IdentDetails';
                document.frm2.action = '../servlet/com.infy.cis.ui.cif.EntityConv_IdentForm_Det?IsFormatReadOnly=Yes';
                document.frm2.submit();
            }
        }
    } catch (e) {
        DebugMessage(e.message);
    }
}
