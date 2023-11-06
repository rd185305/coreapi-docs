<h3>Accounts:</h3>
	userAccountSummary:
		[[PersonAgreementAccountInquiryRequest]]
		[[AccountYTDHistoryInquiryRequest]] (with multiple accounts)
	accountSummary:
		[[PersonAgreementAccountInquiryRequest]]
		[[AccountYTDHistoryInquiryRequest]] (with single account)
	createAccount:
		[[PersonAgreementAccountInquiryRequest]]
		[[AccountMaintenanceRequest]]
		[[PersonOrganizationEAgreementMaintenanceRequest]] (OR) [[PersonAgreementMaintenanceRequest]]
	updateDescription:
		[[AccountMaintenanceRequest]]

<h3>Cards:</h3>
	[[PersonAgreementAccountInquiryRequest]]

<h3>Checks:</h3>
	stopPaymentApply:
		[[AccountStopPaymentMaintenanceRequest]]
	stopPaymentInquiry:
		[[AccountStopPaymentListRequest]]
	checkInquiry:
		[[AccountTransactionHistoryRequest]]

<h3>Customers:</h3>
	userVerify:
		[[PersonAgreementAccountInquiryRequest]] (if **EAgreement** or **enableLoginOnlyForActiveAgreements** is enabled)
		[[PersonDetailInquiryRequest]] (if **CardAgreement**)
	userInfo:
		[[PersonAgreementAccountInquiryRequest]] (if **EAgreement** or **enableLoginOnlyForActiveAgreements** is enabled)
		[[PersonDetailInquiryRequest]] (if **CardAgreement**)
	emailUpdate:
		EAgreement:
			[[PersonAgreementAccountInquiryRequest]]
			[[PersonMaintenanceRequest]]
		CardAgreement:
			[[PersonDetailInquiryRequest]]
			[[PersonMaintenanceRequest]]
	customerUpdate:
		EAgreement:
			[[PersonAgreementAccountInquiryRequest]]
			[[PersonMaintenanceRequest]]
		CardAgreement:
			[[PersonDetailInquiryRequest]]
			[[PersonMaintenanceRequest]]
	userEnroll:
	userLookup:
		serviceType = RETAIL_BANKING:
			[[PersonDetailInquiryRequest]]
			personNumber != null:
				*finish mapping*
			isOrgUsersSupported = true:
				*same as serviceType != RETAIL_BANKING*
		else:
			[[OrganizationSearchRequest]]
			personNumbers != nullOrEmpty:
				[[PersonAgreementInquiryRequest]]
	updatePin: (only for EAgreement)
		[[PersonAgreementAccountInquiryRequest]]
		[[ChangeCardAgreementPINRequest]]

<h3>Transactions:</h3>
	accountHistory:
		[[AccountTransactionHistoryRequest]]
		[[PendingAccountTransactionInquiryRequest]] (if **pending** enabled)
		[[AccountHoldsInquiryRequest]] (if **holds** enabled)
		[[AccountTransactionHistoryRequest]] (as many times till filled)

<h3>Transfers:</h3>
	TRANSFER_TO_GL, TRANSFER_FROM_GL (Zelle):
		[[NOWMonetaryTransactionRequest]]
	TRANSFER, LOC_PAYDOWN, CD_REDEMPTION, LOAN_PAYOFF, NEW_ACCOUNT_FUNDING:
		[[OneTimeTransferMaintenanceRequest]]
		[[AccountBalancesInquiryRequest]]
	TRANSFER_TO_GL (not Zelle), TRANSFER_TO_FI_OWNED:
		[[OneTimeTransferMaintenanceRequest]]
	INTRA_INSTITUTION_TRANSFER:
		transferPin starts with "UNK":
			[[OneTimeTransferMaintenanceRequest]]
		else:
			[[MemberToMemberValidationRequest]]
			if isSuccess:
				transfer.validateToAccountInCrossTransfer = true:
					[[AccountDetailInquiryRequest]]
					[[SystemCodeInquiryRequest]]
					if accountNumberCalculationVariableList != nullOrEmpty && CalculationVariableValue = "N" or "n":
						[[OneTimeTransferMaintenanceRequest]]
						[[AccountBalancesInquiryRequest]]
					else:
						[[SystemCodeInquiryRequest]]
						if accountNumberCalculationVariableList != nullOrEmpty && CalculationVariableValue = "N" or "n":
							[[OneTimeTransferMaintenanceRequest]]
							[[AccountBalancesInquiryRequest]]
				else:
					[[OneTimeTransferMaintenanceRequest]]
					[[AccountBalancesInquiryRequest]]
	verifyOnly:
		TRANSFER, NEW_ACCOUNT_FUNDING, TRANSFER_TO_GL, TRANSFER_TO_FI_OWNED, CD_REDEMPTION:
		LOC_PAYDOWN, LOAN_PAYOFF:
			[[DatabaseHandshakeRequest]]
			[[AccountPayOffPayDownInquiryRequest]]
		INTRA_INSTITUTION_TRANSFER:
			if transferPin = "UNK" && transfer.validateToAccountInCrossTransfer = true
			[[MemberToMemberValidationRequest]]
			if isSuccess:
				[[AccountDetailInquiryRequest]]
				[[SystemCodeInquiryRequest]]
				if accountNumberCalculationVariableList != nullOrEmpty && CalculationVariableValue = "N" or "n":
					[[OneTimeTransferMaintenanceRequest]]
					[[AccountBalancesInquiryRequest]]
				else:
					[[SystemCodeInquiryRequest]]
					if accountNumberCalculationVariableList != nullOrEmpty && CalculationVariableValue = "N" or "n":
						[[OneTimeTransferMaintenanceRequest]]
						[[AccountBalancesInquiryRequest]]