# DSA_ASSIGNMENT01
Explaination : Regex.h 
#ifndef REGEX_H
#define REGEX_H
//those preprocessor directories are used to prevent multiple inclusions of the same header file in a single source file. when you include this header in multiple places, the compiler will see the 'REGEX_H' macro and only include the contents if it hasnt been included before. this prevents duplicate definitions and other potential issues.

#include <string> //this provides the 'string' class used string manipulation.
#include <vector> // the 'vector' class for dynamic arrays.
using namespace std;

class Regex{
public:
    Regex(const string& pattern); => takes a 'const string&' parameter reperesenting the pattern to be serached. 
    vector<int> findPatternPosition(const string& text); takes 'const string&' parameter the text to serach in and returns a vector of integers representing positions where the pattern is found.
private:
    string pattern_;
    to store the pattern
};


#endif // REGEX_H

Regex.cpp
#include "Regex.h"
this file is using the declerations and definitions from the "Regex.h" header file
using namespace std;

Regex::Regex(const string& pattern) : pattern_(pattern){}
implementation of the constructor for the Regex class.
vector<int> Regex::findPatternPosition(const string& text){
    vector<int> positions;
    an empty vector to store the positions of pateern occurence
    size_t pos = 0;
    initializes the pos variable to keep track of the current position in the text
    while(pos < text.length()){
    as lonf as the current position is within the length of the text
        size_t matchPos = text.find(pattern_, pos);
        searches for the 'pattern_' within the 'text' statrting from the current position 'pos'. if a  match is found, the position of the match is  stored in 'matchPos' will be 'string::npos'. 
        if(matchPos != string::npos){
            positions.push_back(static_cast<int>(matchPos));
            adds the position of the match to  the 'poditions' vector.
            pos = matchPos + pattern_.length();
            updates the 'pos' the position after the current match, so the search continues after the match.
        }else{
            break;
        }
    }
    return positions;
}

main.cpp
#include <iostream> 
#include <fstream> provides file stream functionality
#include <vector> dynamic array functionality
#include "Regex.h" 
using namespace std;

int main() {
    ifstream patternFile("pattern.txt");
    ifstream textFile("text.txt");
    opens two files
    string pattern, text;
    if (patternFile.is_open() && textFile.is_open()) { 
    checks if the both files are open sucessfully.
        getline(patternFile, pattern);
        files are open, it reads the pattern from the "pattern.txt" file using 'getline()' and stores it in thr 'pattern' string.
        text.assign(istreambuf_iterator<char>(textFile), istreambuf_iterator<char>());
        Regex regex(pattern);
        vector<int> positions = regex.findPatternPosition(text);
        if (!positions.empty()) {
            cout << "Pattern found at positions: ";
            for (int position : positions) {
                cout << position << " ";
            }
            cout << endl;
        } else {
            cout << "Pattern not found." << endl;
        }
        patternFile.close();
        textFile.close();
    } else {
        cerr << "Error opening files." << endl;
        return 1;
    }
    return 0;
}





