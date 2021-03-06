## [П]|[РС]|(РП) Первая программа - отображение картинки

OpenCV содержит средства для чтения широкого спектра типов изображений, а также видеофайлов с диска и видеопотока с камеры. Эти средства являются частью компонента *HighGUI*, который включен в OpenCV. В дальнейшем, часть этих средств будет использована для создания простой программы - открытие изображения и отображения его на экране (Пример 2-1).

Пример 2-1. Простая OpenCV программа - загрузка изображения с диска и отображение его на экране

```cpp
#include “highgui.h”

int main( int argc, char** argv ) {
    IplImage* img = cvLoadImage( argv[1] );                 // Получение имени изображения
    cvNamedWindow( "DisplayPicture", CV_WINDOW_AUTOSIZE );  // Создание окна
    cvShowImage( "DisplayPicture", img );                   // Показ изображения
    cvWaitKey(0);                                           // Ожидание
    cvReleaseImage( &img );                                 // Освобождение памяти из под изображения
    cvDestroyWindow( "DisplayPicture" );                    // Удаление окна
}
```

После компиляции и запуска из командной строки с одним аргументом, программа загрузит изображение в память и отобразит его на экране. Программа будет ожидать нажатие любой клавиши для закрытия окна и завершения работы. Давайте разберем программу построчно и разберемся, что делает каждая из приведенных строк.

```cpp
IplImage* img = cvLoadImage( argv[1] );
```

Эта строка загружает изображение. Функция *cvLoadImage()* является высокоуровневой: определяет формат файла по его имени; автоматически выделяет память, необходимую для изображения. Обратите внимание, функция *cvLoadImage()* поддерживает широкий спектр типов изображений: BMP, DIB, JPEG, JPE, PNG, PBM, PGM, PPM, SR, RAS и TIFF. Функция возвращает указатель типа **IplImage**. Это наиболее часто используемый тип данных в OpenCV. Используется для обработки всех видов изображений: одноканальные, многоканальные, целочисленные, вещественные и т.д. В дальнейшем манипуляция изображением производиться через созданный указатель **IplImage**.

```cpp
cvNamedWindow( "DisplayPicture", CV_WINDOW_AUTOSIZE );
```

Еще одна высокоуровневая функция *cvNamedWindow()*, открывает окно для размещения и отображения изображения. Функция из библиотеки HighGUI, присваивает имя окну (в нашем случае "DispalyPicture"). Последующие вызовы *HighGUI* будут взаимодействовать с созданным окном по его имени.

Второй аргумент функции *cvNameWindow()* определяет свойства окна. Может быть установлен как 0 (значение по умолчанию) или как *CV_WINDOW_AUTOSIZE*. В первом случае, размер окна не будет зависеть от размера загружаемого изображения, изображение будет подстраиваться под размеры окна. Во втором случае, окно будет подстраиваться под размеры загружаемого изображения. 

```cpp
cvShowImage( "DisplayPicture", img );
```

Всякий раз, когда есть изображение в виде указателя **IplImage**, его можно отобразить в существующем окне с помощью функции *cvShowImage()*. Функция требует, чтобы имя окна уже существовало (создано с помощью *cvNamedWindow()*). Вызов *cvShowImage()* перерисовывает окно, с соответствующим изображением в нем, и изменяет его размеры, если при создании был указан флаг *CV_WINDOW_AUTOSIZE*.

```cpp
cvWaitKey(0);
```
	
Функция *cvWaitKey()* заставляет программу ожидать нажатие любой клавиши. Если аргумент - положительное число, программа будет ожидать заданное количество миллисекунд, а затем продолжит выполнение программы, даже если ничего не будет нажато. Если параметр - 0 или отрицательное число, программа будет бесконечно ожидать нажатие клавиши.

```cpp
cvReleaseImage( &img );
```

Когда изображение больше не требуется, нужно освободить выделенную память. Для выполнения этой операции необходимо передать указатель типа **IplImage***. После выполнения операции, указатель *img* будет установлен в NULL. 

```cpp
cvDestroyWindow( "DisplayPicture" );
```

В завершении, нужно уничтожить окно. Функция *cvDestroyWindow()* закрывает окно и высвобождает любую, связанную с этим окном, память (внутренний буфер для изображения, который содержит копию информации о пикселях из **img**). Для столь простой программы (пример 2-1), можно было и не использовать *cvDestroyWindow()* или *cvReleaseImage()*, т.к. все ресурсы и окна автоматически уничтожаются операционный системой при выходе, однако использование этих функций хорошая привычка.

В итоге имеем простую программу, с которой можно ещё "поиграться" различными способами, но пока не будем забегать вперед. Следующей задачей будет построение почти столь же простой программы - программы для чтения и отображения AVI файла. После этого перейдем к более серьезным вещам.

