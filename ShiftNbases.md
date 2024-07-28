## Challenge

Cháu ngoan Bác Hồ thì phải luôn ghi nhớ 19051890!

![image](https://github.com/user-attachments/assets/0ff2d747-f61e-4475-9350-8d43da26b878)

## Solution

![image](https://github.com/user-attachments/assets/3ee17ea2-0e8c-412b-ab0e-2cd260131d67)

Ta thay o vi tri 0,4,8,... thi cac so thuoc gia tri ` oct ` , vi tri 1,3,5,7,... thi cac gia tri thuoc he ` dec `

Cac vi tri 2 6 10 thuoc he ` hex `

- Hay decode naoo :3

f = open("shiftNbases.txt", "r")
cipher = f.read().split()
f.close()
base = [8, 10, 16, 10]
text = ""
for i in range(0, len(cipher)):
    text += chr(int(cipher[i], base[i % 4]))
    
print(text)

- Ta duoc output

ISP_IN_YOUR_AREAh|phk}Y{SjtdbiVmnr_sf}fi^mez^{ISP_IN_YOUR_AREA`nghrf^kf_p~yjmgYimq_i`v^.^]atISP_IN_YOUR_AREA^pfa^lht^pf_mqusfghuxnndjpfnfhISP_IN_YOUR_AREAan^kf_sqedwid_oqar^llobD_yqwegISP_IN_YOUR_AREA^whzmoVqtes^ip,^lodp}pemhdznkISP_IN_YOUR_AREAVsnwg1^ylydw_ytg[o^adp}pemhmzISP_IN_YOUR_AREAtgZathhfmpVpg~c3^Tfi^kay^pl_`ISP_IN_YOUR_AREA_t^|iomp_g`vVTtes^v^omhDtbgcaISP_IN_YOUR_AREAohnflg(76?_htiVntxcdL%_R~ydqwISP_IN_YOUR_AREAeg^{a1^kXu^jydbwVy^wgmhiVl`C_yISP_IN_YOUR_AREA`|Vc`hcfbg[amhttbgkrdw_ygmVghxISP_IN_YOUR_AREAidcml_rrnm^zX_arnm^lXnf5_i`vVtISP_IN_YOUR_AREAnl_s`wVctwgdbwVqtes^{fnf5_vtISP_IN_YOUR_AREA\n^|usfgjunwgduiVqtes^|l_cx.dISP_IN_YOUR_AREAAie_S~yjmgegnw_Sgie_p~yjmgma^MISP_IN_YOUR_AREAas^ylydw_htiVC`lhdlieg^YhfogeaISP_IN_YOUR_AREAlh1<89Vctwgdmw`:^WgznqVt`hsnmpISP_IN_YOUR_AREAVr`htz^lf_uj_ghv__cjnl^~\_p~yjISP_IN_YOUR_AREAmgcohD_{`ggh`r_qtwe_k~os^llobhISP_IN_YOUR_AREAtz^lf_uj_ghv__cjnl^~\_p~yjmgcoISP_IN_YOUR_AREAh7_Ingca^whzmoVldhpm`qVkgxnl^iISP_IN_YOUR_AREA`_bqon^kXi^mutb6t

- De y ISP_IN_YOUR_AREA khong lien quan

- decode not cho con lai

- Vi de bai la Shift and base nen no la Shifting cipher

![image](https://github.com/user-attachments/assets/b3adc3dc-0372-4405-b3b8-e81fc19dbb36)

Vậy là có liên quan tới con số 19051890 thật. Cụ thể:

`
i - 1 = h
s + 9 = |
...
`

Vậy là các ký tự sẽ được shift theo quy luật dãy số 19051890, xen kẽ - và +.

Tiếp tục decode:

- Ta duoc flag

`
ispclub{Tat_ca_moi_nguoi_deu_sinh_ra_co_quyen_binh_dang._Tao_hoa_cho_ho_nhung_quyen_khong_ai_co_the_xam_pham_duoc;_trong_nhung_quyen_ay,_co_quyen_duoc_song,_quyen_tu_do_va_quyen_muu_cau_hanh_phuc._Loi_bat_hu_ay_o_trong_ban_Tuyen_ngon_Doc_lap_nam_1776_cua_nuoc_My._Suy_rong_ra,_cau_ay_co_y_nghia_la:_tat_ca_cac_dan_toc_tren_the_gioi_deu_sinh_ra_binh_dang,_dan_toc_nao_cung_co_quyen_song,_quyen_sung_suong_va_quyen_tu_do._Ban_Tuyen_ngon_Nhan_quyen_va_Dan_quyen_cua_Cach_mang_Phap_nam_1791_cung_noi:_Nguoi_ta_sinh_ra_tu_do_va_binh_dang_ve_quyen_loi;_va_phai_luon_luon_duoc_tu_do_va_binh_dang_ve_quyen_loi._Do_la_nhung_le_phai_khong_ai_choi_cai_duoc.}
`
