# Biogas_Predict
Create a model to predict biogas generation from analytical data
[synthetic_biogas_dataset.csv](https://github.com/user-attachments/files/20875746/synthetic_biogas_dataset.csv)
# ติดตั้งแพ็กเกจที่จำเป็น (ถ้ายังไม่มี)
install.packages("randomForest")
install.packages("caret")
install.packages("readr")

# โหลดแพ็กเกจ
library(randomForest)
library(caret)
library(readr)

#่อ่านข้อมูล
data <- read_csv("synthetic_biogas_dataset.csv")

#ตรวจสอบข้อมูลเบื้องต้น
str(data)
summary(data)

#แบ่งข้อมูลเป็น training & testing
set.seed(123)
trainIndex <- createDataPartition(data$Biogas_m3_per_day, p = 0.8, list = FALSE)
trainData <- data[trainIndex, ]
testData <- data[-trainIndex, ]

# สร้างโมเดล Random Forest
model <- randomForest(Biogas_m3_per_day ~ ., data = trainData, ntree = 500, importance = TRUE)

# ทำนายผล
predictions <- predict(model, newdata = testData)


# ประเมินผล
r2 <- R2(predictions, testData$Biogas_m3_per_day)
rmse <- RMSE(predictions, testData$Biogas_m3_per_day)


cat("R² =", round(r2, 4), "\n")
cat("RMSE =", round(rmse, 2), "\n")

# แสดงความสำคัญของตัวแปร
importance(model)
varImpPlot(model)
