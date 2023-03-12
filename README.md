# Octave Project 3
 **This Project is an Analysis into CLIFE Investment**
    Tools Used Power BI and Power Query

**Power Query M code**

let
    Source = Excel.Workbook(File.Contents("C:\Users\HP\Desktop\Octave Project\Project 3\Loans.xlsx"), null, true),
    Sheet1_Sheet = Source{[Item="Sheet1",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(Sheet1_Sheet, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"loan_snap_key", Int64.Type}, {"is_current", type logical}, {"start_date", type datetime}, {"end_date", type any}, {"entity_code", type text}, {"loan_key", Int64.Type}, {"business_dateid", Int64.Type}, {"loan_status_key", type text}, {"loan_substatus_key", type text}, {"currency_code", type text}, {"principal_amount", Int64.Type}, {"principal_amount_paid", type number}, {"principal_amount_outstanding", type number}, {"interest_amount_planned", type number}, {"interest_amount_paid", type number}, {"interest_amount_accrued", Int64.Type}, {"penalty_amount", type number}, {"penalty_amount_paid", type number}, {"penalty_amount_waived", type number}, {"charge_amount", type number}, {"charge_amount_paid", Int64.Type}, {"principal_amount_lccy", Int64.Type}, {"principal_amount_paid_lccy", type number}, {"principal_amount_outstanding_lccy", type number}, {"interest_amount_planned_lccy", type number}, {"interest_amount_paid_lccy", type number}, {"interest_amount_accrued_lccy", Int64.Type}, {"penalty_amount_lccy", type number}, {"penalty_amount_paid_lccy", type number}, {"penalty_amount_waived_lccy", type number}, {"charge_amount_lccy", type number}, {"charge_amount_paid_lccy", Int64.Type}, {"principal_amount_eur", type number}, {"principal_amount_paid_eur", type number}, {"principal_amount_outstanding_eur", type number}, {"interest_amount_planned_eur", type number}, {"interest_amount_paid_eur", type number}, {"interest_amount_accrued_eur", Int64.Type}, {"penalty_amount_eur", type number}, {"penalty_amount_paid_eur", type number}, {"penalty_amount_waived_eur", type number}, {"charge_amount_eur", type number}, {"charge_amount_paid_eur", type number}, {"overdue_days", Int64.Type}, {"overdue_days_max", Int64.Type}, {"finish_dateid", Int64.Type}, {"last_repayment_dateid", Int64.Type}, {"write_off_dateid", Int64.Type}, {"fx_date", type date}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type",{"is_current", "end_date", "loan_snap_key", "entity_code"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Removed Columns",{{"start_date", type date}}),
    #"Merged Queries" = Table.NestedJoin(#"Changed Type1", {"loan_key"}, #"Loan Details", {"loan_key"}, "Loan Details", JoinKind.Inner),
    #"Expanded Loan Details" = Table.ExpandTableColumn(#"Merged Queries", "Loan Details", {"refinanced", "interest_rate", "grace_period", "disbursment_dateid", "maturity_dateid", "loan_term", "loan_officer_user_code"}, {"refinanced", "interest_rate", "grace_period", "disbursment_dateid", "maturity_dateid", "loan_term", "loan_officer_user_code"}),
    #"Removed Other Columns" = Table.SelectColumns(#"Expanded Loan Details",{"start_date", "loan_key", "business_dateid", "loan_status_key", "loan_substatus_key", "currency_code", "principal_amount", "principal_amount_paid", "principal_amount_outstanding", "interest_amount_planned", "interest_amount_paid", "interest_amount_accrued", "penalty_amount", "penalty_amount_paid", "penalty_amount_waived", "charge_amount", "charge_amount_paid", "overdue_days", "overdue_days_max", "finish_dateid", "last_repayment_dateid", "write_off_dateid", "fx_date", "refinanced", "interest_rate", "grace_period", "disbursment_dateid", "maturity_dateid", "loan_term", "loan_officer_user_code"}),
    #"Inserted Date" = Table.AddColumn(#"Removed Other Columns", "maturity_date", each Date.From(Text.From([maturity_dateid], "en-GB")), type date),
    #"Inserted Date1" = Table.AddColumn(#"Inserted Date", "disbursement_date", each Date.From(Text.From([disbursment_dateid], "en-GB")), type date),
    #"Last Repayment date" = Table.AddColumn(#"Inserted Date1", "last_repayment_date", each  Date.From(Text.From([last_repayment_dateid], "en-GB")), type date)
in
    #"Last Repayment date"

**Results**
**-** From the Analysis, there was a 5.03% Unpaid Principal which amount to N1.68Bn

**-** It was Discovered that the total Interest paid About N8.6Bn with 18% Interest Rate

**-** The total Penalty waived was N2.65Bn 

**-** There are 4659 people who either paid after Maturity date or did not pay at all

Check the visual for more insight