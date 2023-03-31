# testSearchVendor

import {urlEndpoints, vendorPage} from "../../helper/names";
import {vendorSearch} from "../../helper/selectors";
import {
    clearAndTypeText,
    clickOnEl,
    clickOnElByName,
    clickOnElByText,
    inputNewText,
} from "../../helper/methods";
// import {businessOwner} from "../../helper/testData";
import {interceptVendorSearch} from "../../helper/graphql-test-utils";

describe('Worker search on the Worker Page', () => {
    before('Create Vendors', () => {
        cy.apiLogin('businessowner15@businessowner.com', 'Aa123123');
        cy.visit(urlEndpoints.vendorEndpoint);
        cy.deleteAllItemsApi('vendor', )
        cy.createFewItemsApi('vendor', 2)
    });

     beforeEach('Login', () => {
         cy.apiLogin('businessowner5@businessowner.com', 'Aa123123');
         cy.visit(urlEndpoints.vendorEndpoint);
     });

    after(() => {
            // cy.deleteFewItemsApi('vendor', 2)
        })

    const verifyCellFromSearchField = (index, selector) => {
        cy.get(vendorSearch.vendor).eq(index).invoke('text').then((text) => {
            for (let i = 1; i < 6; i++) {
                cy.get(selector).should('be.visible').type(text.slice((i - 1), i))
                interceptVendorSearch()
                // cy.wait(2000)
            }

            cy.get(vendorSearch.dashboardTableCell).contains(text)
            cy.get(vendorSearch.vendorSearchField).should('be.visible').clear({force: true})
        })
    }

    it.only('adding mobile numbers to vendors', () => {
        clickOnElByName(vendorSearch.vendor, vendorPage.vendorCode2)
        clickOnEl(vendorSearch.editIcon)
        clickOnElByText(vendorSearch.vendorTable, vendorPage.addPhoneNumber)
        clearAndTypeText(vendorSearch.mobilePhone, '13054843555')
        clickOnElByText(vendorSearch.editVendorTable, vendorPage.updateButton)
        cy.visit(urlEndpoints.vendorEndpoint);
        clickOnElByName(vendorSearch.vendor, vendorPage.vendorCode1)
        clickOnEl(vendorSearch.editIcon)
        clickOnElByText(vendorSearch.vendorTable, vendorPage.addPhoneNumber)
        clearAndTypeText(vendorSearch.mobilePhone, '17865467641')
        clickOnElByText(vendorSearch.editVendorTable, vendorPage.updateButton)
    })

    it('Verify search vendor by Code', () => {
        verifyCellFromSearchField(0, vendorSearch.vendorSearchField)
    })

    it('New Verify search vendor by Name', () => {
        verifyCellFromSearchField(1, vendorSearch.vendorSearchField)
    })

    it.only('New Verify search vendor by Email', () => {
        verifyCellFromSearchField(2, vendorSearch.vendorSearchField)
    })

    it.only('New Verify search vendor by Mobile Number', () => {
        verifyCellFromSearchField(3, vendorSearch.vendorSearchField)
//         cy.intercept('POST', '/graphql', (req) => {
//             if (req.body.includes('VendorSearch')) {
//                 req.alias = 'gqlQuery'
//             }
//         })
//
// // assert that a matching request has been made
//         cy.wait('@search')
    })


    it('Search for non-existing element', () => {
        clickOnEl(vendorSearch.vendorSearchField)
        inputNewText(vendorSearch.vendorSearchField, 'gsdsdgds')
        cy.get(vendorSearch.vendorPageTable).should('not.exist')
    });
})
