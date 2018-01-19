# utl_US_Address-standarization
US address standarization.  
    Standardizing Addresses

    SAS/WPS/R SAS/WPS/Python:

    US address standarization

    SAS/WPS/Python: Address standarization

    github
    https://github.com/rogerjdeangelis/utl_US_address-standarization

    inspired by
    http://tinyurl.com/jmfxmg9
    https://communities.sas.com/t5/Base-SAS-Programming/Distinguishing-an-actual-City-value-rather-than-a-false-positive/m-p/322975

    see
    https://goo.gl/DR99j4
    https://communities.sas.com/t5/Base-SAS-Programming/Standardizing-Addresses/m-p/429255

    The python package below is not a panacea.
    You still need to apply rules to clean up
    indeterminate addresses.

    But I have found the following package to
    be very useful for US addresses. They may have other
    languages.

    HAVE (send it the entire address as one string, or
    just the text without city, state and postal code.
    Other tools to fix city, state and zip if you can hand parse.)
    them.


    ==============================================================

    HAVE
    ====

    ENROLLMENT GROUP 39 E COCONUT ST APT 2 Chicago IL 20906


    WANT
    ====

    [
    (u'ENROLLMENT', 'Recipient'),
    (u'GROUP', 'Recipient'),
    (u'39', 'AddressNumber'),
    (u'E', 'StreetNamePreDirectional'),
    (u'COCONUT', 'StreetName'),
    (u'ST', 'StreetNamePostType'),
    (u'APT', 'OccupancyType')
    (u'2\n', 'OccupancyIdentifier'),
    (u'Chicago\n', 'CityName')
    (u'IL\n', 'StateName')
    (u'20906\n', 'PostalCode')
    ]


    SOLUTION

    * the Python USADDRESS package can do much more than
    this. But lets see if it can parse the 6 permutations
    of the same address below. I have already used PERL
    to do some translations , ie East to E, Street to ST, THIRD to 3rd
    floor to FL.

    %utl_submit_py64(%nrbquote(
    fo = open('d:/txt/KeyBadAdrFix.txt', 'w');
    import usaddress;
    for line in open('d:/txt/KeyBadAdr.txt'):;
    .    addr=line[11:];
    .    key=line[0:11];
    .    res=key + str(usaddress.parse(addr)) + '\n';
    .    lyn=str(addr);
    .    fo.write(str(res));
    ));
