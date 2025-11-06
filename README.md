Блок для чтения данных с устройств по протоколу Modbus TCP для ПЛК Siemens 400 серий через CP443.
Основной особенностью является поддержка нескольких запросов по одному соединению, а так же максимальная гибкость, открытость, простота использования.
Блок написан на языке SCL.

Поддерживает функции 1,2,3,4.

Возможна работа как обычных ПЛК, так и на резервируемых (H-серии).
Поля CP_LADDR_1 и ID_1 заполняются только для случаев использования в H-системе (указываются разные CP_LADDR) или при необходимости резервирования каналов.
При потере связи блок перебирает соединения пока не найдет рабочее. 

Для обеспечения возможности запрашивать несколько областей/функций по одному соединению необходимо вызвать несколько экземпляров и указать в ioIdent/ioBusy аналогичные поля предыдущего блока. Для первого блока указываются значения последнего вызываемого блока.
Если достаточно одного вызова, то поля ioIdent/ioBusy остаются пустыми.

Полученное значение записывается в выхода outReg(можно заменить на собственную структуру)

Для диагностики соединения используется выход NOCONNECT.

Пример использования:
MODBUS_TCP_Reader.DB1(Timeout :=T#3s  // IN: TIME
                        ,CP_LADDR_0 := 16#3FF9 // IN: WORD
                        ,ID_0 := 2  // IN: INT
                        ,CP_LADDR_1 := 16#3FF9 // IN: WORD
                        ,ID_1 := 3 // IN: INT
                        ,MDBS_ADDR := 1 // IN: INT
                        ,MDBS_FUNC := 3 // IN: INT
                        ,MDBS_NUM_OFFSET := 0 // IN: INT
                        ,MDBS_NUM_REG := 5 // IN: INT
                        ,ioIdent := DB3.ioIdent // INOUT: INT
                        ,ioBusy := DB3.ioBusy // INOUT: BOOL
                        ); 
MODBUS_TCP_Reader.DB2(Timeout :=T#3s  // IN: TIME
                        ,CP_LADDR_0 := 16#3FF9 // IN: WORD
                        ,ID_0 := 2  // IN: INT
                        ,CP_LADDR_1 := 16#3FF9 // IN: WORD
                        ,ID_1 := 3 // IN: INT
                        ,MDBS_ADDR := 1 // IN: INT
                        ,MDBS_FUNC := 3 // IN: INT
                        ,MDBS_NUM_OFFSET := 10 // IN: INT
                        ,MDBS_NUM_REG := 10 // IN: INT
                        ,ioIdent := DB1.ioIdent // INOUT: INT
                        ,ioBusy := DB1.ioBusy // INOUT: BOOL
                        ); 
MODBUS_TCP_Reader.DB3(Timeout :=T#3s  // IN: TIME
                        ,CP_LADDR_0 := 16#3FF9 // IN: WORD
                        ,ID_0 := 2  // IN: INT
                        ,CP_LADDR_1 := 16#3FF9 // IN: WORD
                        ,ID_1 := 3 // IN: INT
                        ,MDBS_ADDR := 1 // IN: INT
                        ,MDBS_FUNC := 1 // IN: INT
                        ,MDBS_NUM_OFFSET := 0 // IN: INT
                        ,MDBS_NUM_REG := 5 // IN: INT
                        ,ioIdent := DB2.ioIdent // INOUT: INT
                        ,ioBusy := DB2.ioBusy // INOUT: BOOL
                        );

MIT License

Copyright (c) 2025 Alexandr Kataev 

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.