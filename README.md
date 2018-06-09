# utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job
ODS Graphics look at Chicago Public Schools.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    ODS Graphics look at Chicago Public Schools

    see for github graphics
    https://tinyurl.com/ya2wcdb3
    https://github.com/rogerjdeangelis/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job/blob/master/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job

    SAS Forum
    https://tinyurl.com/ybwx9ato
    https://communities.sas.com/t5/SAS-GRAPH-and-ODS-Graphics/Top-40-A-SAS-ODS-Graphics-look-at-Chicago-Public-Schools/m-p/468240

    Tc profile
    https://communities.sas.com/t5/user/viewprofilepage/user-id/4628


    INPUT
    =====

    https://tinyurl.com/y8p9555q
    https://github.com/rogerjdeangelis/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job/blob/master/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job


    PROCESS
    =======

    libname xel "d:/xls/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job.xls";
    data salary;
      set xel.'Export Worksheet$'n;
    run;quit;
    libname xel clear;

    proc means data=salary nway n mean median p25 p75;       /* Gather descriptive stats for each job title */
    class Job_Title; var Annual_Salary;                       /* Count, mean, median, 25th/75th percentiles */
    output out=job_title_stats n=N mean=MEAN median=MEDIAN p25=P25 p75=P75;
    run;quit;

                                                                 /* Rank job titles based on # of employees */
    proc rank data=job_title_stats out=job_title_stats_rank ties=low descending;
    var n; ranks nrank;
    run;quit;

    proc sql;                     /* Select individual salaries for top 40 job titles, based on # employees */
    create table employees_rank as
    select s.annual_salary, r.* from salary s join job_title_stats_rank r on s.Job_Title=r.Job_Title
    where r.nrank<=40 order by r.median desc;
    ;quit;

    options orientation=landscape;
    ods pdf file="d:/pdf/&pgm..pdf" notoc;
    ods graphics on / reset width=17in height=11in ANTIALIASMAX=34500;    /* Show distributions w/box & scatter plots*/

    proc sgplot data=employees_rank noautolegend;
    title "Chicago Public Schools: Base Annual Salary Distribution by Job Title (Top 40 Job Titles, Based on Number of Employees)";
    hbox Annual_Salary / category=Job_Title Fill noOutliers boxwidth=.7 transparency=.75; /* 1st HBOX with fill (background) */
    scatter y=Job_Title x=Annual_Salary / transparency=.75;                               /* 2nd HBOX without fill, but w/red medan markers */
    hbox Annual_Salary / category=Job_Title noFill medianattrs=(thickness=2pt) noOutliers boxwidth=.7  meanattrs=(symbol=circlefilled color=red);
    xaxis grid display=(nolabel);
    yaxis discreteorder=data display=(nolabel);
    yaxistable N / stat=mean location=inside position=left /*label="#"*/;    /* Stats on every obs, so specify stat=mean */
    yaxistable median / stat=mean location=inside position=left /*label="Med Salary"*/;
    yaxistable mean / stat=mean location=inside position=left /*label="Avg Salary"*/;
    yaxistable p25 / stat=mean location=inside position=left /*label="P25"*/;
    yaxistable p75 / stat=mean location=inside position=left /*label="P75"*/;
    format n comma7. mean median p25 p75 dollar12.0;
    footnote "Source: Chicago Public Schools Employee Position Files, http://cps.edu/About_CPS/Financial_information/Pages/EmployeePositionFiles.aspx";
    run;quit;

    proc format;
    picture pct low-high='009.9%'; /* Custom format to add % sign to histogram y-axis values */

    ods graphics on / reset width=11in height=8.5in ANTIALIAS;    /* Show distribution for 'Regular Teachers' w/histogram */
    proc sgplot data=employees_rank(where=(job_title="Regular Teacher"));
    title "Chicago Public Schools: 'Regular Teacher' Salary Distribution";
    histogram Annual_Salary / datalabel=count showbins;
    xaxis display=(nolabel) fitpolicy=rotate;
    yaxis display=(nolabel) valuesformat=pct.;
    format annual_salary dollar12.0;
    footnote "Source: Chicago Public Schools Employee Position Files, http://cps.edu/About_CPS/Financial_information/Pages/EmployeePositionFiles.aspx";
    run;quit;

    ods pdf close;


    OUTPUT
    ======

    see for github graphics
    https://tinyurl.com/ya2wcdb3
    https://github.com/rogerjdeangelis/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job/blob/master/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job



