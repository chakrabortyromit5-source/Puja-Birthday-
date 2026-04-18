import React, { useMemo, useState } from 'react'; import { Card, CardContent } from '@/components/ui/card'; import { Button } from '@/components/ui/button'; import { Textarea } from '@/components/ui/textarea';

const rules = [ {cat:'Delivery Issue', keys:['delivery','parcel','arrived','arrive','package','late','delay','courier','delivered']}, {cat:'Product Quality', keys:['damaged','damage','broken','battery','quality','color','colour','defect']}, {cat:'Technical Issue', keys:['app','website','login','crash','slow','promo','code','checkout']}, {cat:'Customer Service', keys:['support','customer care','reply','calls','rudely','waiting','emailed']}, {cat:'Payment Problem', keys:['charged','payment','paid','money','cancelled']}, {cat:'Refund / Return', keys:['refund','return','credited','rejected']}, {cat:'Account Problem', keys:['account','blocked','unable to login']}, {cat:'Pricing Issue', keys:['price','pricing','different']}, ];

function analyzeText(text){ const t=text.toLowerCase(); const found=[]; rules.forEach(r=>{ if(r.keys.some(k=>t.includes(k))) found.push(r.cat); }); const cats=[...new Set(found.length?found:['General Complaint'])]; const priority = cats.length>1 || t.length>90 ? 'High':'Medium'; const sentiment = /bad|worst|angry|fraud|terrible/.test(t)?'Negative':'Neutral'; return {cats,priority,sentiment}; }

export default function App(){ const [text,setText]=useState(''); const [history,setHistory]=useState([]); const result = useMemo(()=> text? analyzeText(text):null,[text]); const submit=()=>{ if(result) setHistory([result,...history].slice(0,5)); }; return <div className='p-6 max-w-6xl mx-auto space-y-6'>

 <h1 className='text-3xl font-bold'>AI Complaint Analyzer - Deploy Ready</h1>
 <p className='text-sm text-gray-500'>Paste complaint text in English / mixed language for instant classification.</p>
 <Card className='rounded-2xl'><CardContent className='p-4 space-y-4'>
 <Textarea rows={6} value={text} onChange={e=>setText(e.target.value)} placeholder='Example: money deducted but order not confirmed' />
 <div className='flex gap-2'><Button onClick={submit}>Analyze</Button><Button variant='outline' onClick={()=>setText('')}>Clear</Button></div>
 </CardContent></Card>
 {result && <Card className='rounded-2xl'><CardContent className='p-4 space-y-2'>
 <div><b>Categories:</b> {result.cats.join(', ')}</div>
 <div><b>Priority:</b> {result.priority}</div>
 <div><b>Sentiment:</b> {result.sentiment}</div>
 <div><b>Auto Reply:</b> We apologize. Your issue has been routed to the relevant team.</div>
 </CardContent></Card>}
 <div className='grid md:grid-cols-2 gap-4'>
 <Card className='rounded-2xl'><CardContent className='p-4'><div className='font-semibold mb-2'>Recent Analyses</div>{history.map((h,i)=><div key={i} className='text-sm'>{h.cats.join(', ')} - {h.priority}</div>)}</CardContent></Card>
 <Card className='rounded-2xl'><CardContent className='p-4'><div className='font-semibold mb-2'>Deploy Steps</div><ol className='text-sm list-decimal pl-5'><li>Upload to GitHub</li><li>Import in Vercel</li><li>Click Deploy</li></ol></CardContent></Card>
 </div>
 </div>
}
