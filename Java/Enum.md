## Enum 을 문자열로 변환

```java
public enum DateFormat {

    YYYYMMDD {
        @Override
        public String toString() {
            return "yyyy-MM-dd";
        }
    }

}
```

```java
facilityApplyVo.setDay(DateParserUtils.getDateDay(date, DateFormat.YYYYMMDD.toString()));
```


## Example

```java
@GetMapping("/{year}")
public String year(@ModelAttribute("budgetVo") MecBudgetVo budgetVo, \
                   @PathVariable Integer programSeq,
                   RedirectAttributes redirectAttributes) {
    switch (findByYear(budgetVo.getYear())) {
        case YEAR_2015 :
            return BudgetsJSP.JSP_2015.getReturnUrl();
        case YEAR_2014 :
            return BudgetsJSP.JSP_2014.getReturnUrl();
        case YEAR_2013 :
            return BudgetsJSP.JSP_2013.getReturnUrl();
        case YEAR_2012 :
            return BudgetsJSP.JSP_2012.getReturnUrl();
        case YEAR_2011 :
            return BudgetsJSP.JSP_2011.getReturnUrl();
        case YEAR_2010 :
            return BudgetsJSP.JSP_2010.getReturnUrl();
        case YEAR_2009 :
            return BudgetsJSP.JSP_2009.getReturnUrl();
        case YEAR_2008 :
            return BudgetsJSP.JSP_2008.getReturnUrl();
    }
    redirectAttributes.addFlashAttribute("message", Message.DATA_ACCESS_ERROR.getMsg());
    return "redirect:/kor/"+programSeq+"/budget";
}
```

```java
@Getter
public enum BudgetsJSP {

    JSP_2015("kor/budget/2015.sub"),
    JSP_2014("kor/budget/2014.sub"),
    JSP_2013("kor/budget/2013.sub"),
    JSP_2012("kor/budget/2012.sub"),
    JSP_2011("kor/budget/2011.sub"),
    JSP_2010("kor/budget/2010.sub"),
    JSP_2009("kor/budget/2009.sub"),
    JSP_2008("kor/budget/2008.sub");

    private String returnUrl;

    BudgetsJSP(String returnUrl) {
        this.returnUrl = returnUrl;
    }

}
```

```java
@Getter
public enum BudgetsYear {
    YEAR_2015("2015"),
    YEAR_2014("2014"),
    YEAR_2013("2013"),
    YEAR_2012("2012"),
    YEAR_2011("2011"),
    YEAR_2010("2010"),
    YEAR_2009("2009"),
    YEAR_2008("2008");

    private String year;

    BudgetsYear(String year) {
        this.year = year;
    }

    public static BudgetsYear findByYear (String year) {
        for (BudgetsYear budgetsYear : BudgetsYear.values()) {
            if (budgetsYear.equals(year)) {
                return budgetsYear;
            }
        }
        throw new RuntimeException();
    }
}
```
