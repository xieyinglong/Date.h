#ifndef __DATE_H__
#define __DATE_H__
#include <string>
#include <sstream>
using namespace std;
class Date {
    public:
        Date() {}
        Date(int y, int m, int d, int h, int mi) {
            year_ = y;
            month_ = m;
            day_ = d;
            hour_ = h;
            minute_ = mi;
        }
        int getYear(void) const {
            return year_;
        }
        void setYear(int year) {
            year_ = year;
        }
        int getMonth(void) const {
            return month_;
        }
        void setMonth(int month) {
            month_ = month;
        }
        int getDay(void) const {
            return day_;
        }
        void setDay(int day) {
            day_ = day;
        }
        int getHour(void) const {
            return hour_;
        }
        void setHour(int hour) {
            hour_ = hour;
        }
        int getMinute(void) const {
            return minute_;
        }
        void setMinute(int minute) {
            minute_ = minute;
        }
        static bool isValid(Date date) {
            if (!(date.getYear() >= 1000 && date.getYear() <= 9999))
                return false;
            if (!(date.getMonth() >= 1 && date.getMonth() <= 12))
                return false;
            if (date.getMonth() != 2) {
                switch (date.getMonth()) {
                    case 1:
                    case 3:
                    case 5:
                    case 7:
                    case 8:
                    case 10:
                    case 12:
                        if (!(date.getDay() >= 1 && date.getDay() <= 31))
                            return false;
                        break;
                    case 4:
                    case 6:
                    case 9:
                    case 11:
                        if (!(date.getDay() >= 1 && date.getDay() <= 30))
                            return false;
                        break;
                    default:
                        break;
                }
            }
            else if (Isleap(date)) {
                if (!(date.getDay() >= 1 && date.getDay() <= 29))
                    return false;
            }
            else {
                if (!(date.getDay() >= 1 && date.getDay() <= 28))
                    return false;
            }
            if (!(date.getHour() >= 0 && date.getHour() <= 23))
                return false;
            if (!(date.getMinute() >= 0 && date.getMinute() <= 59))
                return false;
            return true;
        }
        static Date stringToDate(std::string dateString) {
            stringstream ss;
            int y, m, d, h, mi;
            char t;
            ss << dateString;
            ss >> y >> t >> m >> t
               >> d >> t >> h >> t >> mi;
            return Date(y, m, d, h, mi);
        }
        static std::string dateToString(Date date) {
            stringstream ss;
            ss << date.getYear() << "-"
               << supplyZero(date.getMonth()) << "-"
               << supplyZero(date.getDay()) << "/"
               << supplyZero(date.getHour()) << ":"
               << supplyZero(date.getMinute());
            return ss.str();
        }
        Date &operator=(const Date& date) {
            setYear(date.getYear());
            setMonth(date.getMonth());
            setDay(date.getDay());
            setHour(date.getHour());
            setMinute(date.getMinute());
            return *this;
        }
        bool operator==(const Date& date) const {
            return getYear() == date.getYear() &&
                   getMonth() == date.getMonth() &&
                   getDay() == date.getDay() &&
                   getHour() == date.getHour() &&
                   getMinute() == date.getMinute();
        }
        bool operator>(const Date& date) const {
            return getYear() > date.getYear() ? true :
                   getMonth() > date.getMonth() ? true :
                   getDay() > date.getDay() ? true :
                   getHour() > date.getHour() ? true : false;
        }
        bool operator<(const Date& date) const {
            return !(*this > date) && !(*this == date);
        }
        bool operator>=(const Date& date) const {
            return !(*this < date);
        }
        bool operator<=(const Date& date) const {
            return !(*this > date);
        }
    private:
        int year_;
        int month_;
        int day_;
        int hour_;
        int minute_;
        static bool Isleap(Date date) {
            return (date.getYear() / 4 == 0 && date.getYear() / 100 != 0)
                   || date.getYear() / 400 == 0;
        }
        static string supplyZero(int arg) {
            stringstream ss;
            if (arg < 10) {
                ss << "0" << arg;
            }
            else {
                ss << arg;
            }
            return ss.str();
        }
};
#endif
