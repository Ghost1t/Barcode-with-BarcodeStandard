using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.IO;
using System.Linq;
using System.Text;
using System.Drawing;
using System.Threading.Tasks;
using System.Windows.Forms;
using TFlex.DOCs.Model.Macros;
using TFlex.DOCs.Model.Macros.ObjectModel;
using TFlex.DOCs.Model.References.Files;

using BarcodeStandard;

public class Macro : MacroProvider
{
    public Macro(MacroContext context)
        : base(context)
    {    	
        //System.Diagnostics.Debugger.Launch();
        //System.Diagnostics.Debugger.Break();
    }

    public override void Run()
    {
    	BarcodeLib.Barcode b = new BarcodeLib.Barcode();
    	object ID = ТекущийОбъект.Параметр["ID"];
    	string ID_Объекта = Convert.ToString(ID);
        Image imageBarcode = b.Encode(BarcodeLib.TYPE.CODE128B, ID_Объекта, Color.Black, Color.White, 290, 120);
        ТекущийОбъект.Изменить();
        ImageConverter converter = new ImageConverter();
        var result = (byte[])converter.ConvertTo(imageBarcode, typeof(byte[]));// Преобразуем изображение кода в массив байт
        ТекущийОбъект.Параметр["2953c159-40f0-4549-9de8-e6c2d0d1f6ac"] = result;
        ТекущийОбъект.Сохранить();
        
        
        string file_name = ТекущийОбъект.Параметр["Наименование"] + ".png";
        //       
        int Слэш = file_name.IndexOf("/");
        if (Слэш >= 0)
        	{
        file_name = file_name.Remove(Слэш, 1);
        }
        Объект Папка = НайтиОбъект("Файлы", "Наименование", "Штриховые коды");
        Объект Image = СоздатьОбъект("Файлы", "PNG Image", Папка); 
        Image.Параметр["Наименование"] = file_name;      
        Image.Сохранить();        
        
        ТекущийОбъект.Изменить();
        ТекущийОбъект.Подключить("Штриховой код", Image);
        ТекущийОбъект.Сохранить();
        var fileReference = new FileReference(Context.Connection);
        var folder = fileReference.FindByRelativePath("Штриховые коды") as FolderObject;        
        FileObject file_Object = fileReference.AddFile(file_name, result, folder, false);
        
    
    }
       
}
