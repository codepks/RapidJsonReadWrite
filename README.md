# RapidJsonReadWrite
Reading from an exiting json file with C++ and updating the key value and making a new file

rapidjson.h header file is needed in the same directory and needs to be installed separately from vcpkg.
I just copied the header file from the vcpkg sub directory and placed int the same folder where I am testing my code.

test.json file has nested structure which I am trying to read via ifstream.
laters= using IStreamWrapper:

IStreamWrapper wraps any class derived from std::istream, such as std::istringstream, std::stringstream, 
std::ifstream, std::fstream, into RapidJSON's input stream.

Since only streams can be parsed to Document and later:

    Document doc {};
    doc.ParseStream( isw );


check this link for reference: https://rapidjson.org/md_doc_stream.html

After parsing the stream, I am using StringBuffer:
StringBuffer is a simple output stream. It allocates a memory buffer for writing the whole JSON. Use GetString() to obtain the buffer.

using .GetString() from the buffer to print the json contents:
    
    const std::string jsonStr { buffer.GetString() };
    std::cout << jsonStr << '\n';
    
You can update the key and values using this:
    
    doc[ "hello" ] = "127.0.0.1";
    
 Writing to a new files is done via:
    
    std::ofstream ofs { R"(NewTest.json)" };
    if ( !ofs.is_open() )
    {
        std::cerr << "Could not open file for writing!\n";
        return EXIT_FAILURE;
    }

    OStreamWrapper osw { ofs };
    Writer<OStreamWrapper> writer2 { osw };
    doc.Accept( writer2 );
    
For iterating through the json file use:
    
    for (Value::MemberIterator itr = doc.MemberBegin(); itr != doc.MemberEnd(); ++itr)
    {
       std::cout<<itr->name.GetString(); //Key  
    }
    
