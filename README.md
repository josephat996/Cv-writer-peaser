#include <iostream>
#include <fstream>
#include <string>
#include <regex>

void parseCV(const std::string& fileName) {
    std::ifstream file(fileName);
    if (!file.is_open()) {
        std::cerr << "Error: Could not open file " << fileName << std::endl;
        return;
    }

    std::string line;
    std::string name, email, phone;
    std::regex emailRegex(R"(([\w\.-]+)@([\w\.-]+)\.(\w+))");
    std::regex phoneRegex(R"(\b(\+?\d{1,3}[-.\s]?)?(?\d{1,4}?[-.\s]?)?\d{1,4}[-.\s]?\d{1,4}[-.\s]?\d{1,9}\b)");
    
    while (std::getline(file, line)) {
        // Extract Name (Assume first non-empty line is the name)
        if (name.empty() && !line.empty()) {
            name = line;
        }

        // Extract Email
        std::smatch emailMatch;
        if (std::regex_search(line, emailMatch, emailRegex)) {
            email = emailMatch.str();
        }

        // Extract Phone Number
        std::smatch phoneMatch;
        if (std::regex_search(line, phoneMatch, phoneRegex)) {
            phone = phoneMatch.str();
        }
    }

    file.close();

    // Print extracted details
    std::cout << "Name: " << (name.empty() ? "Not Found" : name) << std::endl;
    std::cout << "Email: " << (email.empty() ? "Not Found" : email) << std::endl;
    std::cout << "Phone: " << (phone.empty() ? "Not Found" : phone) << std::endl;
}

int main() {
    std::string fileName;
    std::cout << "Enter the CV file name: ";
    std::cin >> fileName;

    parseCV(fileName);

    return 0;
}