# utl_US_Address-standarization
    Parse us postal addresses into components

    see github
    https://tinyurl.com/y7au8ple
    https://github.com/rogerjdeangelis/utl_US_address-standardization/blob/master/utl_US_address-standarization.sas

    see
    https://tinyurl.com/yddmy7rw
    https://communities.sas.com/t5/SAS-Statistical-Procedures/Efficient-code-for-split-street-address-information/m-p/478572

    see
    https://goo.gl/zuvHFU
    https://communities.sas.com/t5/Base-SAS-Programming/Separate-Apt-information-from-Street-information/m-p/363139

    inspired by
    http://tinyurl.com/jmfxmg9
    https://communities.sas.com/t5/Base-SAS-Programming/Distinguishing-an-actual-City-value-rather-than-a-false-positive/m-p/322975

    The python package below is not a panacea.
    You still need to apply rules to clean up
    indeterminate.  It also handles recipiient.


    INPUT
    =====

     d:/txt/utl_parse_addresses_into_components.txt

       REC1 39 E COCONUT ST APT 2 CHICAGO IL 20906
       REC2 22 EAST STREET APT 2A MIDLAND IL 20906
       REC3 BOX 1422 HOLLYWOOD CA 20906


    EXAMPLE OUTPUT
    --------------

       REC1[
       (u'39', 'AddressNumber'),
       (u'E', 'StreetNamePreDirectional'),
       (u'COCONUT', 'StreetName'),
       (u'ST', 'StreetNamePostType'),
       (u'APT', 'OccupancyType'),
       (u'2', 'OccupancyIdentifier'),
       (u'CHICAGO', 'PlaceName'),
       (u'IL', 'StateName'),
       (u'20906\n', 'ZipCode')]


    PROCESS
    =======


    %utlfkil(d:/txt/utl_parse_addresses_into_components_out.txt); * delete;

    %utl_submit_wps64("
    options set=PYTHONHOME 'C:\Progra~1\Python~1.5\';
    options set=PYTHONPATH 'C:\Progra~1\Python~1.5\lib\';
    libname sd1 'd:/sd1';
    proc python;
    submit;
    fo = open('d:/txt/utl_parse_addresses_into_components_out.txt', 'w');
    import usaddress;
    for line in open('d:/txt/utl_parse_addresses_into_components.txt'):;
    .    addr=line[4:];
    .    key=line[0:4];
    .    res=key + str(usaddress.parse(addr)) + '\n';
    .    lyn=str(addr);
    .    fo.write(str(res));
    endsubmit;
    run;quit;
    ");


    OUTPUT  (reformatted)
    ======

       REC1[
       (u'39', 'AddressNumber'),
       (u'E', 'StreetNamePreDirectional'),
       (u'COCONUT', 'StreetName'),
       (u'ST', 'StreetNamePostType'),
       (u'APT', 'OccupancyType'),
       (u'2', 'OccupancyIdentifier'),
       (u'CHICAGO', 'PlaceName'),
       (u'IL', 'StateName'),
       (u'20906\n', 'ZipCode')]

       REC2[(u'22', 'AddressNumber'),
       (u'EAST', 'StreetName'),
       (u'STREET', 'StreetNamePostType'),
       (u'APT', 'OccupancyType'),
       (u'2A', 'OccupancyIdentifier'),
       (u'MIDLAND', 'PlaceName'),
       (u'IL', 'StateName'),
       (u'20906\n', 'ZipCode')]

       REC3[(u'BOX', 'USPSBoxType'),
       (u'1422', 'USPSBoxID'),
       (u'HOLLYWOOD', 'PlaceName'),
       (u'CA', 'StateName'),
       (u'20906\n', 'ZipCode')]

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data _null_;
     length address $64;
     file "d:/txt/utl_parse_addresses_into_components.txt";
     input;
     address=_infile_ ;
     put address;
    cards4;
    REC1 39 E COCONUT ST APT 2 CHICAGO IL 20906
    REC2 22 EAST STREET APT 2A MIDLAND IL 20906
    REC3 BOX 1422 HOLLYWOOD CA 20906
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process

